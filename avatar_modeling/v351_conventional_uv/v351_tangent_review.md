> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v351 새 UV·blend shape tangent 기술 검토

## 판정

**제안한 기본면 및 각 shape frame의 새 tangent 재계산은 타당하다.** 새 UV에서 basis tangent만 바꾸고 옛 shape tangent delta를 붙이는 방식은 기준 좌표가 달라져 잘못된 결과가 된다. 다만 아래 handedness·sparse 보존·저장 후 복원 검사를 함께 적용해야 한다. 이번은 읽기 전용 설계 검토이며 새 구현 실행 합격 판정은 아니다.

실제 `trials/unity_build.log`는 Coat에서 nonzero shape tangent 12,071건을 발견해 저장 완료 전에 예외로 차단했다. 원본 프리팹의 Coat mesh GUID `6faf5ff45fc4544cab5b93f6173b2253`는 `Assets/BiancaTextureResolutionReview/Bianca_v343_TextureResolution_Bianca_Coat.asset`다. 해당 원래 serialized 데이터를 별도로 읽어 **32 shape frame, sparse entry 총12,984건, nonzero tangent12,071건, hasTangents=1 frame26개/0 frame6개**를 확인했다. 12,071은 프레임별 sparse 건수 합계이며 고유 정점 개수가 아니다.

## 재계산 방법

새 UV와 최종 destination 정점 분리/삼각형/재질 경계를 확정한 mesh를 기준으로 다음을 계산한다.

```text
P0, N0, UVnew, triangles → T0 = RecalculateTangents
각 원래 shape frame f:
    Pf = P0 + exact source deltaPosition[sourceIndex]
    Nf = normalize(N0 + exact source deltaNormal[sourceIndex])
    Tf = RecalculateTangents(Pf, Nf, UVnew, same triangles)
    deltaTf.xyz = Tf.xyz − T0.xyz
```

원래 기본 좌표·노멀·shape position/normal delta는 최종 자산에서 변경하지 않는다. normalize는 tangent 계산용 scratch mesh의 frame normal에만 적용한다. 프레임 full vector에 basis를 더하는 방식과 full vector에서 basis를 빼 delta를 만드는 방식은 Unity API의 명시된 의미와 일치한다. [GetBlendShapeFrameVertices](https://docs.unity3d.com/2022.3/Documentation/ScriptReference/Mesh.GetBlendShapeFrameVertices.html), [AddBlendShapeFrame](https://docs.unity3d.com/2022.3/Documentation/ScriptReference/Mesh.AddBlendShapeFrame.html)

`RecalculateTangents`는 위치·노멀·UV를 사용하며 기존 normal을 재계산하는 호출이 아니다. 이 프로젝트는 custom normal 보존이 필요하므로 `RecalculateNormals`를 추가하지 않는다. scratch mesh에는 필요한 모든 배열과 원래 실제 삼각형을 반드시 제공한다. 누락된 normals/UV/triangles이면 fallback tangent(1,0,0,1)을 만들 수 있으므로 유한값만 확인해서는 충분하지 않다. Float32 형식 변환 가능성도 API에 명시돼 있어 저장 후 원래 P/N 보존 검사를 유지한다. [Unity RecalculateTangents](https://docs.unity3d.com/2022.3/Documentation/ScriptReference/Mesh.RecalculateTangents.html)

32개 프레임의 기존 weight/name/order를 그대로 유지한다. 각 저장 frame은 그 프레임의 delta를 직접 더한 full shape이며, frameWeight/100을 다시 곱한 좌표로 재계산하지 않는다. 여러 frame을 가진 shape로 일반화할 때도 누적 delta로 처리하지 않는다.

## 반드시 필요한 handedness 검사

기본 tangent는 Vector4이고 w는 binormal 방향 부호다. `B = cross(N,T.xyz) * T.w`이며 w는 ±1이다. 반면 blend shape의 deltaTangents API는 Vector3이므로 w delta를 저장하지 못한다. [Unity Mesh.tangents](https://docs.unity3d.com/2022.3/Documentation/ScriptReference/Mesh-tangents.html)

따라서 **각 frame·각 destination 정점의 `Tf.w == T0.w`**를 검사한다. 다르면 xyz delta만으로 정확한 프레임 tangent basis를 재현할 수 없다. 해당 key/정점/면/재질을 기록하고 자동 합격을 막는다. w를 조용히 basis 값으로 강제하거나 xyz만 비교해 “정확”으로 기록하지 않는다. 퇴화 면, 뒤집히는 변형, custom normal 방향과 geometry 불일치가 원인일 수 있으므로 원인을 구분한다.

또한 각 Pf/Nf/Tf의 유한값, Nf 정규화 전 길이, tangent 길이, `dot(Nf,Tf.xyz)`를 검사한다. normal+delta가 거의 0인 곳은 무조건 normalize해서 0을 넘기지 말고 실패 위치를 남긴다. UV 퇴화/근접 퇴화도 분리한다. 모든 결함을 원래 geometry 수정으로 자동 확대할 필요는 없다.

## Sparse 데이터 재작성

기존 v343 `TextureResolutionViews.cs`와 현 `ConventionalUVViews.cs::CopyExactShapes`는 native AddBlendShapeFrame의 작은 delta 축약을 피하기 위해 `m_Shapes`를 직접 보존하는 경로다. 원래 Coat 자산에는 1e−11 수준 position delta도 실제 존재한다. 새 tangent를 만든다고 이 P/N 값을 임계값으로 0 처리하면 원래 보존 요구를 위반한다.

권장 순서:

1. 원래 `m_Shapes.channels`, name/nameHash, frameIndex/frameCount, fullWeights 및 frame 플래그를 복사한다.
2. 각 원래 sparse entry의 vertex/normal float와 source index를 destination 복제 정점에 정확 대응시킨다.
3. **전체 destination 정점**에서 새 delta tangent를 계산한다. 원래 P/N 변화가 없는 정점도 이웃 면이 변하면 tangent는 바뀔 수 있다.
4. 기존 entry 집합과 새 tangent-only entry 집합의 합집합을 만든다. 기존 entry는 새 tangent가 0이어도 제거하지 않는다. 새 tangent-only entry의 P/N delta는 정확히 0으로 둔다.
5. 한 프레임 안에서 destination index 중복을 없애고 index 순서로 정렬한다. `firstVertex`, `vertexCount`를 다시 계산하고 tangent가 있는 frame의 hasTangents를 true로 둔다. 기존 hasNormals는 불필요하게 변경하지 않는다.
6. destination sourceIndex 역매핑이 없는 원래 entry는 조용히 누락하지 말고 검사를 실패시킨다.

`hasTangents=false`였던 원래 6개 프레임도 새 UV에서 geometry/normal 변화에 따른 tangent delta가 필요할 수 있으므로 계산 대상에서 제외하지 않는다. 새 tangent delta 자체를 normalize하면 안 된다. 두 단위 tangent의 **차이 벡터**다.

원래 source entry의 xyz 복사를 마친 뒤 옛 `CopyExactShapes`를 다시 호출하면 새 tangent가 옛 것으로 덮인다. 또한 `EditorUtility.CopySerialized`·UV 재할당·최종 RecalculateTangents 이후 실제 basis가 T0와 같은지 확인한다. 현 코드 후반에는 저장 mesh에 UV/triangles를 다시 설정하고 RecalculateTangents를 한 번 더 수행하므로 이 순서에 주의한다.

`m_Shapes` 내부 SerializedProperty는 공개 Mesh API의 안정적 계약이 아니다. 현재 Unity 2022.3.22f1의 필드 구조를 명시적으로 검사하고, 누락/형식 변경 시 실패시키는 방식으로 사용한다. 이는 Editor 제작 과정이며 runtime 커스텀 컴포넌트를 추가할 이유는 없다.

## 프레임 endpoint와 실제 혼합의 차이

개별 frame endpoint에서는 T0+ΔTf가 새 UV의 Tf를 재구성하도록 만들 수 있다. 그러나 중간 weight에서는 Unity가 tangent delta를 선형 보간/합산한다. 일반적으로

```text
RecalculateTangents(P0 + a·ΔP, normalize(N0+a·ΔN))
```

는 `normalize(T0 + a·ΔT)`와 정확히 같지 않다. 여러 key를 동시에 적용할 때도 비선형성 때문에 endpoint delta를 합친 결과와 실제 결합 geometry에서 재계산한 tangent가 다를 수 있다. 이것은 제안 방식 자체를 부정하는 것은 아니며, 일반 blend shape 표현의 범위다.

따라서 최종 설명은 **“원래 각 shape frame에서 새 UV 기준 tangent를 재산출했다”**로 한정한다. 실제로 사용되는 팔꿈치 보정의 25/50/75/100% 및 대표 동시 조합, 신발에 영향을 주는 보정 키를 선정하여 사본 렌더/각도 오차를 비교한다. 중간/조합 오차가 0이라는 조건으로 불필요하게 모든 프레임을 실패시키기보다 크기와 시각 영향을 기록한다.

## 저장 후 검증 기준

- 원래 기본 P/N, 모든 shape 이름·개수·frameWeight, P/N sparse delta, weights/bindposes/source mapping이 동일.
- 새 basis tangent finite, 길이·직교성·w 정상; 각 endpoint w 불일치0.
- 저장 후 재로드의 `GetBlendShapeFrameVertices`에서 tangent xyz가 계산한 ΔTf와 일치; P/N은 source mapping 대비 정확히 동일.
- sparse 원래 entry 보존 건수, 새 tangent-only 건수, 새 전체 entry 건수, old/new hasTangents frame 수를 따로 기록.
- 원래 m_Shapes의 channels/fullWeights와 renderer의 현재 shape weights도 동일.
- 저장 후 UV/triangles/vertex count 및 basis tangent가 재계산 입력과 동일.
- 실제 평탄 노멀맵/선택 신발 노멀맵으로 정면·측면·관절 shape 시점 비교. tangent 변경을 texture 색 변경이나 geometry 변경으로 섞어 설명하지 않는다.

Blender에서 새 UV tangent normal을 bake했다는 사실과 Unity RecalculateTangents의 basis가 시각적으로 일치한다는 사실은 별도다. 두 API가 동일 알고리즘이라고 공식 문서 없이 단정하지 않는다. 최종 Unity의 동일 triangulation/노멀/UV seam/재질 분리 상태에서 선택 노멀맵과 실제 렌더를 확인한다. 현재 Coat normal map이 비활성이어도 tangent 보존 문제를 무시해서는 안 되며, 미래 normal/aniso 효과나 shape 일관성을 위해 올바른 데이터로 전달하는 것이 적절하다.

## 증거 위치

- `avatar_modeling/v351_conventional_uv/ConventionalUVViews.cs`
- `avatar_modeling/v351_conventional_uv/trials/unity_build.log`
- `avatar_modeling/v343_texture_resolution/TextureResolutionViews.cs`
- `unity_vrchat_avatar/Assets/BiancaRemainingNpr/Bianca_v349_WorldWind.prefab`
- `unity_vrchat_avatar/Assets/BiancaTextureResolutionReview/Bianca_v343_TextureResolution_Bianca_Coat.asset`

이번 검토는 위 파일을 읽고 공식 Unity 2022.3 API 문서와 대조했다. 프로젝트/Unity 상태는 변경하지 않았다. 신규 재계산 구현·수치/렌더 합격은 아직 이 검토에서 실행하지 않았다.

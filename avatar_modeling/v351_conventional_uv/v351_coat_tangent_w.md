> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v351 Coat tangent.w 독립 기술 검토

기존 형상키/관절 형상을 바꾸지 않고 새 UV의 기본색을 쓰되 Coat의 기존 tangent basis와 tangent delta를 정확 보존하는 제한된 NPR 경로는 타당합니다. 그러나 이것은 **새 UV 기준 tangent normal 지원 완료가 아니며**, 해당 기능은 명시적으로 보류해야 합니다. 기존 모양을 고쳐서 이 검사만 통과시키는 방향은 이번 범위에 맞지 않습니다.

## 실제 Unity 결과

제작자가 실행한 `avatar_modeling/v351_conventional_uv/ConventionalUVViews.cs:60`의 `AnalyzeTangents`와 `COAT_TANGENT_DIAGNOSIS.json`을 독립적으로 읽었습니다. 검토자는 Unity 명령이나 프로젝트 변경을 하지 않았습니다.

- Coat 원래 3,894정점/3,413삼각형/32개 단일 프레임 키를 사용합니다. source_index는 완전한 identity이며 기존/새 triangle 배열도 정확히 같습니다.
- 기존 UV와 새 UV 각각 basis부터 Unity `RecalculateTangents`를 실행하고, 각 키의 원래 position/normal delta를 적용한 종단 프레임을 다시 재계산하여 해당 basis와 w를 비교합니다. 원래 가져온 tangent와 직접 비교한 수치는 아닙니다.
- 원래 UV **77건**, 새 UV **77건**입니다. 건수는 `키 프레임 × 정점`이며 각각 59개/61개 고유 정점, 18개/18개 키에 걸쳐 있습니다. 양쪽 공통 71건, 기존만 6건, 새 UV만 6건입니다.
- near_zero_normal은 64기록 모두 0입니다.

|새 UV에서만 발생한 키|source 정점|
|---|---|
|V112_side.R|2323|
|V152_volume120.R|107, 2323|
|V152_volume60.R|2633|
|V333_ElbowContour_L|3529|
|V333_ElbowContour_R|1080|

기존만 발생한 것은 V137_back.L:1653, V152_volume60.R/90.R/120.R:167, V332_ElbowFold_GL:587, V333_ElbowContour_L:1628입니다. 따라서 “77개가 새 UV 때문에 생겼다”와 “원래도 77이므로 정확히 같은 문제다”는 모두 부정확합니다.

## 독립 기하 계산과 원인 범위

`/tmp/v351_coat_tangent_w.py`는 원본 Unity asset의 실제 Float32 position/normal/tangent/UV stream과 UInt32 삼각형, serialized sparse 키를 읽었습니다. geometry 변경/저장 없이 `P+deltaP`, `normalize(N+deltaN)`에서 각 면 cross product와 해당 코너 shading normal의 내적 부호를 검사했습니다.

새 Unity w 변화 77건 중 **73건**이 기존 키를 적용했을 때 위 상대 부호가 바뀌는 인접 면에 연결되어 있습니다. 새 UV만 발생한 6건도 모두 이 범위에 들어갑니다. 전체 키-면-코너 기록에서 부호 변화는 229건이며, 이 기하 계산에는 UV를 사용하지 않습니다.

각 삼각형 UV 미분 tangent/bitangent는 `cross(T,B)=cross(edge1,edge2)/det(UV)` 관계를 갖습니다. 형상키의 접힘과 유지/변형된 shading normal의 상대 방향 변화가 handedness 변화에 기여한다는 강한 근거입니다. 새 UV는 동일 정점에 모이는 tangent 방향과 크기를 바꾸므로 동일 geometry에서도 6건이 없어지고 6건이 생길 수 있습니다.

다만 73건의 인접성은 각 사례의 인과를 완전히 분해한 증명이 아닙니다. 나머지 4건은 V152_volume60.L/90.L의 정점1653, V152_volume90.R/120.R의 정점882입니다. 이들은 직접 인접 면의 상대 부호 변화가 없어 tangent fan 평균/상쇄 또는 그룹화의 기여를 추가 분리해야 합니다. 전체를 “메시가 뒤집혔다”라고 단정하면 안 됩니다. 이것은 키 단독 100%에서 face normal과 제공 normal의 관계 검사이며, 실제 본 포즈/키 혼합 결과의 관통·시각 불량 판정이 아닙니다.

32개 키 단독 종단 프레임의 최소 면적비는 원래 대비 **0.05009794**(V112_side.R)이며 완전한 0면적은 없습니다. 원래/새 UV 삼각형의 최소 절대 determinant도 각각 약6.75e-7/1.18e-6으로 0이 아닙니다. 따라서 정확한 UV 퇴화나 0-normal 하나로 77건을 설명할 수 없습니다. 접힘/강한 압축 및 normal 관계가 남습니다.

Python의 보통 UV 미분 누적(uniform/각도 가중)도 기존 UV부터 여러 w 변화를 재현합니다. 그러나 basis에서 가져온 tangent.w와 이미 86/90개가 다르므로 **이 근사값을 Unity/MikkTSpace의 정확한 정점 판정으로 쓰지 않았습니다.** 확정 77/77은 실제 Unity JSON에서만 인용했습니다.

## 보존 방안의 필수 조건

1. Coat의 basis Vector4 tangent 및 원래 32키의 전체 sparse position/normal/tangent delta/has flags를 정확 보존하고 새 UV와 색 텍스처만 적용합니다. Coat 재계산 tangent를 중간 단계 또는 최종 재로드 단계에서 덮어쓰면 안 됩니다.
2. 실제 저장된 최종 Coat 재질에서 `_UseBumpMap=0`, `_UseBump2ndMap=0`, 대응 texture null 및 tangent를 사용하는 anisotropy/MatCap/parallax/outline vector 등의 비활성을 검증합니다. shader 기능에 따라 tangent 사용 여부를 읽고 판단해야 하며 단순 파일 이름으로 보장하지 않습니다.
3. **원래 Coat 재질은 `_UseBumpMap=1`**입니다. 원래 v005 selective normal의 Coat 영역이 중립이라는 상태와 새 Coat에서 기능 자체를 껐다는 변경을 구분해 기록합니다. 원래부터 bump가 꺼져 있었다고 쓰면 안 됩니다.
4. 이 fallback은 기존 NPR 표현 보존용이며 새 UV normal authoring용 basis가 아닙니다. 향후 Coat tangent normal을 켜기 전에 handedness 표현/normal delta/키 조합 문제를 별도로 해결해야 합니다. BodyHair/신발의 새 tangent+normal 검증과 합쳐서 “모든 부위 normal 재구축 완료”라고 하면 안 됩니다.
5. 종단 tangent 재구축이 가능한 BodyHair도 키 중간값/혼합값의 tangent는 endpoint 차분 선형 보간이므로 실제 non-linear 재계산과 항상 일치하지 않습니다. 기능을 쓰는 부위와 실제 조합에서 확인해야 합니다.

## Unity API 제약

Base tangent는 Vector4이고 w가 binormal 방향 부호를 담지만 blendshape tangent delta는 Vector3입니다. 따라서 basis 대비 w가 바뀌는 프레임은 delta.xyz만 재작성해서 정확히 표현할 수 없습니다. w를 강제로 basis에 맞추면 계산 검사는 통과해도 새 UV의 tangent frame을 정확 재현했다는 주장은 성립하지 않습니다.

- [Mesh.tangents](https://docs.unity3d.com/2022.3/Documentation/ScriptReference/Mesh-tangents.html)
- [Mesh.AddBlendShapeFrame](https://docs.unity3d.com/2022.3/Documentation/ScriptReference/Mesh.AddBlendShapeFrame.html)
- [Mesh.RecalculateTangents](https://docs.unity3d.com/2022.3/Documentation/ScriptReference/Mesh.RecalculateTangents.html)

## 재현 및 증거

검토 전용 실행: `.venv/mac_python/bin/python /tmp/v351_coat_tangent_w.py`

- `/tmp/v351_coat_tangent_w.json`: Python 기하/근사 및 실제 Unity 77/77 대조 결과. 스크립트 재실행은 JSON의 별도 exact_unity 집계를 덮어쓰므로 현재 확정본을 보존할 것.
- `/tmp/v351_coat_tangent_geometry.npz`: 읽은 basis 좌표/normal/기존·새 UV/삼각형.
- 원본: `unity_vrchat_avatar/Assets/BiancaTextureResolutionReview/Bianca_v343_TextureResolution_Bianca_Coat.asset`.
- 실제 Unity 근거: `avatar_modeling/v351_conventional_uv/COAT_TANGENT_DIAGNOSIS.json`.

이 검토는 read-only 수학/코드/기록 대조입니다. 최종 fallback asset 저장 및 재로드 보존 검사, 실제 최종 재질 확인, 실제 관절 연속 렌더 합격을 대신하지 않습니다.

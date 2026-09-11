> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v167 — Unity PC 통합·실제 SDK 검증 작업 중

## 최신 결과 — 이후 기록보다 이 절이 우선

- **자동24키 구동 확인:** SimpleDirectional2D 단위원과 정확한 피크 각도로 수정.32자세 최대 키 오차0.0008106%, 별도174자세0.0021458%. `FX_POSE_CHECK.json`, `FX_HOLDOUT2_CHECK.json`이 유효하다. 실제 SDK Contacts와 Animator가 값을 생성했으며 검사 코드가 키 값을 직접 넣지 않았다. 정적 정확도이며 연속 동작/IK/스케일은 v170에서 이어 검사한다.
- 홀드아웃 첫155개 후 반복 JSON 덮어쓰기에서 IO1224가 발생했다. 원인을 로그로 확인하고155개를 보존하여 나머지19개를 재개했다. `FX_HOLDOUT_CHECK.json`은 첫155개, `FX_HOLDOUT2_CHECK.json`이 전체174개다. 이 파일 오류를 모델/보정 실패로 세지 않는다.
- **작은 웨이트 복원 완료:** Unity 기존 메시 사본에 Blender 본 이름별 원래4영향을 복원했다. 최대 가중치 L1오차 BodyHair1.56e-7/Coat1.53e-7. 정점·노멀·UV0·삼각면 보존 검사 통과. 해당 스크립트는 전체 UV레이어/모든 키 좌표 비교까지 수행한 것은 아니다. `WEIGHT_RESTORE_*.json`.
- **회전 보간 오차 개선 확인:** 손목마다 중간 회전3 Transform을 추가하여65% 구간을 분할했다. 실제제약24개. 수정 후32자세 VRC 제약/Blender 최대정점차 BodyHair0.001403mm/Coat0.040119mm. 원래 소매 약0.451mm 차이에서 크게 감소했다. `REFINED_POSE_CHECK.json`. Blender에서 모든 본을 직접 적용한 BAKED도 코트0.040119mm 잔차가 있어, 이를 전부 제약 오류라고 부르지 않는다. SDK 내부의 보간 알고리즘 자체를 확인했다는 주장은 하지 않는다.
- **바람 메뉴/레이어 구성:** `BiancaWind` 0–1, 기본0.35, 헤어6/옷4부모10초주기 애니메이션, Expressions 메뉴 Breeze. FX에4번째 wind레이어와 변환 마스크를 추가했다. 통합 후 동작은 v170 검사 중이며 실제 월드 WindZone 자동수신은 아니다.
- Contacts94개는 공식 PC Contacts Poor 상한32를 초과하여 해당 항목 Very Poor다. 자동 보정을 원격에서도 계산하려고 Local Only로 제한하지 않았다. 실제 비용과 대안 검토가 남아 있다.

아래는 원인과 수정 경로를 남긴 이전 단계 기록이다. ‘검토 중’은 이 최신 결과와 구분한다.

**아직 통합 완료 파일이 아니다.** 별도 `bianca_DYNAMICS_PREP.blend`는 v165 소매 후보에 옷자락 바람 부모4/끝점4와 넥타이 끝점1을 더했다.129본, 기존32773정점·원형 유지. Unity 테스트는 `.venv/reference_pose_unity` Unity2022.3.22f1+SDK3.10.5에서 실행한다. 실제 VRChat 클라이언트/Build & Test/업로드는 수행하지 않는다.

## 현재 확인한 것

- Humanoid51본 매핑, avatar.isValid/isHuman true. 이것만으로 T-pose·IK 품질을 통과했다고 하지 않는다.
- 실제 VRC 로컬 회전 제약18개. Physics11개=기존헤어6+옷자락4+넥타이1. 분리 Renderer2개.
- Blender32자세를 Unity로 변환해 전체본을 직접 적용한 BAKED와 실제 VRC CONSTRAINTS를 비교했다. 모양키 값은 이 검사에서 명시적으로 공급했다. 자동FX 검사는 별도다.
- 초기 BAKED 정점 최대차 BodyHair0.131mm/Coat0.183mm. 실제제약 최대차0.437/0.451mm. 기존0.5제약은 각도지표0, 새 소매0.65제약은 최대0.582° 차이. 회전 보간 구현 차이 가능성을 검사 중이다.
- Unity 기본 가져오기에서 원래 작은 웨이트가 생략되는 현상: 정점별 L1최대 BodyHair0.00477/Coat0.00332. Custom4/최소1e-8 설정도 실제 .meta에서는0.001로 남았다. 원형 좌표 불일치로 오해하지 않는다. 원래 웨이트 보존 후처리 여부 검토 중.

원본과 가져온 정점은 rest 위치와 **본 이름별 웨이트 차이**를 함께 사용해 대응했다. BodyHair31042/Coat3603 imported vertices는 UV·노멀 경계 복제 때문이다. 대응 rest최대차0.00048mm. 실제 SDK 동작을 단순히 본 이름의 존재로 판단하지 않았다. `IMPORT_MATCH.json`, `UNITY_POSE_CHECK.json`, `EXPORT.json` 참고.

## 실제 통합 물리 검사

`IntegrationDynamics`로 ON/NoColliders/OFF 각각720프레임. 머리·목·몸통·팔·손목·다리·루트 이동을 섞고 바람 부모를 움직인 뒤 정지 복귀한다. Animator는 끄고 시험 입력을 직접 적용했다. 기본 걷기/Humanoid IK나 자동 보정FX 검사는 이 실행과 다르다.

| 항목 | BodyHair | Coat |
|---|---:|---:|
| 동일 프레임 OFF 대비 최대 표면 차이 |26.545mm|12.306mm|
| 물리로 추가된 면적비0.2미만/3초과 사건 |0|0|
| 처음 정지 차이 |0.000075mm|0.000121mm|
| 끝 정지 차이 |0.000011mm|0.000060mm|
| Collider ON/OFF 최대 표면 차이 |8.983mm|12.306mm|

61개 표면 샘플/구성으로 측정한 범위다. BodyHair는 헤어와 넥타이를 함께 포함하므로26.5mm를 헤어 이동량이라고 부르지 않는다. 실제 본 최대 로컬회전 ON 헤어17.76°,코트11.86°,넥타이5.46°; OFF모두0. 충돌 해결 완료는 면적 경고0과 별개이며 관통·자락 실루엣을 추가 확인한다. `DYNAMICS_SUMMARY.json`, `DYNAMICS_FRAMES.json`. 원시좌표/30사진은 로컬 `.venv/bianca_integrated_motion`.

## Blender 자세 보정의 허용 컴포넌트 이식

기존24키는 모두 코트에 실제 변위가 있으며 BodyHair 복사키는0변위다. 자동 구동은 SDK Contacts와 Animator만 사용하도록 시험 중이다. 임의 런타임 C#을 아바타에 넣어 해결했다고 주장하지 않는다.

본의 서로 직교하는 세 방향을 자기 Contact로 측정한다. 같은 길이 r의 방향점 간 거리 d의 제곱 합은 quaternion pose distance와 대응한다: `sum(d_i²)/(8r²)=1-dot(q,qGoal)²`. 원래 Blender 식의 커널·몸통 게이트를 구성하고, 상완 볼륨은 행렬성분의 방향으로 각도별 가중치를 계산한다. `FX_PLAN.json`은8측정프레임/20회전거리/26게이트/24출력을 명시한다. 현재24Sender+70Receiver=94Contacts,135Animator float,3레이어,679개이상내부에셋이다. 성능 비용이 있으며 최적화 검토가 필요하다. 로컬전용으로 성능 집계를 숨기지 않았다.

초기 단일 커널40방향 시험에서39개는 잘 맞았지만, 한 큰 회전은30프레임이 실제 SDK 갱신을 기다리기에 부족해 이전값이 남았다. 초기결과 `MATH_INITIAL_TIMING.json`을 보존한다. 따라서40개 전부통과로 보고하지 않는다. 실제 통합 검사는 자세별 충분한시간을 두도록 수정했다.

첫 통합은 Direct Blend Tree의 Write Defaults OFF에서 값이 누적되고0복귀가 실패했다. `FX_INITIAL_FAILURE.json`. 공식 문서가 Direct Blend Tree에 Write Defaults ON을 권장하는 예외를 확인하고 전체 세 상태를ON으로 수정했다. 이후32자세에서 대부분 키 오차수ppm 수준으로 맞았으나 Cartesian 각도보간은볼륨 피크에서최대5.58%오차였다. `FX_CARTESIAN_CHECK.json`. 방향/반경을 구분하는 FreeformDirectional과 정확한 피크 각도를 넣은 대체안을 검증 중이다. **아직 자동FX최종통과라고 하지 않는다.**

## 추가 조사 근거

- [Unity ModelImporter skinWeights](https://docs.unity3d.com/cn/2022.3/ScriptReference/ModelImporter-skinWeights.html), [최소 웨이트](https://docs.unity.cn/2023.3/Documentation/ScriptReference/ModelImporter-minBoneWeight.html): 기준 미만 영향 삭제. 실제 설치 버전 결과를 별도 확인.
- [Unity 공식 Rig Editor 소스](https://github.com/Unity-Technologies/UnityCsReference/blob/master/Modules/AssetPipelineEditor/ImportSettings/ModelImporterRigEditor.cs): UI 최소0.001범위. [Mesh.SetBoneWeights](https://docs.unity.cn/ScriptReference/Mesh.SetBoneWeights.html): 저장 정밀도가 입력float와 같지 않을 수 있음.
- [VRChat Rotation Constraint](https://creators.vrchat.com/common-components/constraints/vrc-rotation-constraint/), [Constraints API](https://creators.vrchat.com/common-components/constraints/constraints-api/): 실제18제약에공개API사용.
- [VRChat Contacts](https://creators.vrchat.com/common-components/contacts/): 자기접촉/태그/근접도/비동기 파라미터와 비용. 일반물리Collider와다름.
- [VRChat Write Defaults 지침](https://creators.vrchat.com/avatars/#write-defaults-on-states): Direct와Additive는ON권장, 다른상태OFF와의해당예외명시.
- [Unity2D Blend Tree](https://docs.unity.cn/Documentation/Manual/BlendTree-2DBlending.html): Directional은같은방향의여러속도/반경을구분하며원점클립필요.

Blender MCP의 첫 export는 Edit/Object 전환 및 선택 컨텍스트가 없어 실패했다. 명시temp_override로 수정 후 출력했다. 에러로그는 진행 이력이며 export성공과 구분된다. 구매모델은 로컬검증프로젝트에만 있고 새패키지/Git재배포하지 않는다.

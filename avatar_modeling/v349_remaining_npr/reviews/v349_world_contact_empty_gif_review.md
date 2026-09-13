> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v349 WorldContact 빈 GIF 원인과 안전한 재실행

확정된 직접 원인은 카메라가 실제 런타임 아바타 위치를 보지 않았다는 것입니다. 기존 PNG120장이 모두 단일 RGB(38,41,46) 배경색으로만 구성돼 있습니다. GIF 인코더 문제가 아니라 렌더 원본부터 빈 화면입니다.

## 수치 근거

`world_wind/world_usage/RUN.json`의4800프레임×35개 hair/tie Transform=168,000점에 카메라 frustum을 적용했습니다.

- 고정 카메라: (0.35,0.79,2), target(0,0.79,0), orthographicSize0.32,600×800.
- 수직 가시 범위: **Y0.47–1.11m**.
- 실제 전체 hair/tie 본의 Y범위: **−0.012105–0.330895m**.
- 카메라 안에 들어오는 추적 본: **0/168,000점**.
- hair_wind_00: prefab 월드Y0.9450→runtime 프레임0 Y0.3308954.
- tie_anchor: prefab 월드Y0.7690→runtime Y0.1548954.
- hair00 전체 체인과 tie 전체에서 공통 이동량은 약 **(−0.000902632,−0.61410462,−0.01287602)m**입니다. 이것은 체인의 작은 물리 굽힘과 다릅니다.
- hair_wind_00/tie_anchor 위치는 기록된4800프레임 동안 각각 완전히 고정됩니다. 초기에 이동한 상태로 안정된 런입니다.

prefab의 serialized root는position0/rotationidentity/scale1입니다. 따라서 잘못 저장된 prefab 원점이나 카메라 계산의100scale 혼동은 아닙니다. parent Transform의 모든 TRS를 독립 합성하여 위 prefab 월드좌표를 계산했습니다.

## 기술적 원인의 확정 범위

`MacWorldWindAudit`는 Instantiate 후 Animator AlwaysAnimate만 설정하고 HumanPoseHandler를 만들거나 root/기준 pose를 복원하지 않습니다. `MacRemainingSecondaryAudit`는 이를 수행합니다. 현재 prefab Animator의 `m_ApplyRootMotion=1`이며 humanoid Avatar와 FX성격의 커스텀 controller를 직접 재생합니다. 첫 Pose measurements 레이어는 humanoid mask 전부0이고 root Transform도 mask0입니다. 해당 state는 WriteDefaults1입니다.

공통 번역 이동과 런타임 초기화 코드 차이를 보면 **Editor humanoid Animator 초기 평가에서 body/root 위치가 기준에서 이동했는데 고정 카메라와 pose 없는 테스트가 이를 감지하지 못한 것**이 가장 근거 있는 원인입니다. 다만 기존 RUN에는 root/hips/head/chest 및 Animator.bodyPosition, 첫 Animator 평가 전/후가 없어서 아래 중 정확히 어떤 내부 단계가−0.614m를 만들었는지는 확정하지 못합니다.

- root motion 적용으로 root가 이동했는지,
- root는 그대로이고 humanoid body/hips 재구성으로 skeletal subtree가 이동했는지,
- 초기 controller/default pose 평가의 어떤 단계인지.

따라서 “applyRootMotion=true 하나가 확정 원인” 또는 “PhysBone 설정 때문에 모델이 내려갔다”라고 쓰면 안 됩니다. 실제 기록은 모든 체인에 거의 동일한 초기 이동이 있다는 것까지 입증합니다.

## 권장 재실행 수정 — 테스트 사본/하네스만

1. 기존 비어 있는 영상과 RUN을 trial로 그대로 보존하고, 새로운 출력 폴더를 사용합니다. prefab 제작자산은 이 문제를 숨기기 위해 위치를 올려 저장하지 않습니다.
2. `MacRemainingSecondaryAudit.StartNext`의 초기화 순서를 재사용합니다. inactive container 아래 사본 생성→root0/identity→activate→Animator AlwaysAnimate/Normal→HumanPoseHandler로 첫 애니메이션 평가 전 기준 HumanPose 확보. 실제 Head/Chest/Hips/Foot 위치와 HumanPose.bodyPosition을 즉시 기록합니다. 이 시점부터 이미 이동돼 있으면 초기화 실패로 중단합니다.
3. 하네스에 `[DefaultExecutionOrder(-20000)]`와 LateUpdate를 두고, root0/identity 및 저장된 HumanPose만 복원합니다. wind anchor/hair/tie bone rotation 전체를 저장값으로 매 프레임 덮어쓰면 Animator 바람과 PhysBone을 지우므로 하지 않습니다. SetHumanPose는 기존 하네스처럼 humanoid 기준 복원에만 사용합니다.
4. camera는 SDK완료 후 **실제 head/chest와 head–foot 높이**를 사용해 구도를 정합니다. 고정0.79m를 제거합니다. 머리/넥타이 각각 근접 뷰와 전신 증거1뷰를 권장합니다. 근접/원거리 clip도 .01/30처럼 명시합니다. 모델이 부당하게 내려갔는데 카메라만 따라가 합격시키지 않도록 기준 pose 높이 검사를 함께 둡니다.
5. renderer.updateWhenOffscreen=true를 명시하고 baked mesh 월드 bounds 또는 최소한 Head/Chest viewport 좌표가 화면 안인지 먼저 검사합니다. 실제PNG의 단일색/비배경픽셀 비율 검사로 빈화면을 즉시 실패 처리합니다.
6. SDK실제 ContactSender/tag/World Usage Editor 어댑터와 기존 manual0/.35/.7/1 구간은 보존합니다. World Usage는 enable후SDK가Avatar로 덮어쓰는 이전 문제를 고려해 Update에서World로 보장하고 매프레임 기록합니다. sender를 receiver 실제 중심으로 이동시키는 기존 방식도 보존합니다.
7. 카메라용 수정으로 pose frame 순서가 달라진 새 실행이므로 이전 bone JSON에 새 영상만 붙이지 않습니다. 새로운 RUN의 구간별rx→Animator 반응, 잘못된tag/중복sender/이탈, 수동값 보존을 같이 재검증합니다.
8. 프레임0 전/후에는 root.position/rotation, rig.bodyPosition/bodyRotation, hips/head/chest 위치를 Update전후/LateUpdate복원전후/SDK완료에 기록하면 초기 이동 단계를 분리할 수 있습니다. 전체4800프레임에는 unity_frame/delta_time/root/head/chest/receiver transform/카메라값을 포함하면 재현 해석이 명확해집니다.

applyRootMotion을 테스트에서 끄고 비교하려면 **초기화 전에 고정한 별도 원인 분리 후보**로 수행해야 합니다. Unity 문서는 런타임 변경 시 Animator가 재초기화된다고 명시합니다. 본 실행 중 토글하면 다른 초기조건이 섞입니다. 현존 정상 Remaining 하네스는 매 프레임 pose 복원으로 동작하므로 먼저 그 경로를 그대로 재사용하는 것이 최소 변경입니다. [Unity Animator.applyRootMotion](https://docs.unity3d.com/2022.3/Documentation/ScriptReference/Animator-applyRootMotion.html).

HumanPoseHandler.SetHumanPose는 생성 시 연결한 humanoid 계층의 pose를 설정합니다. bodyPosition/bodyRotation과 muscles를 처음 확보한 기준 그대로 유지하고, 추가 world translation을 pose/bodyPosition에 중복 적용하지 않아야 합니다. [Unity HumanPoseHandler.SetHumanPose](https://docs.unity3d.com/2022.3/Documentation/ScriptReference/HumanPoseHandler.SetHumanPose.html).

## 기존 결과에서 남는 유효성

기존 World Usage/tag 접촉→receiver→Animator 값 반응은 실제SDK기록으로 남습니다. 빈 영상 때문에 해당 수치가 모두 거짓이 되는 것은 아닙니다. 그러나 기존 GIF/PNG는 시각 검수 증거로 사용할 수 없고, 모델이 정상 기준 pose였다는 근거도 아닙니다. 영상 없는 수치 시험과 새 정상 pose 시각 시험을 구분해야 합니다. 실제 VRChat Client/호환 월드 실행 검증은 여전히 별도입니다.

## 증거 파일

- `/tmp/v349_world_camera_review.py`
- `/tmp/v349_world_camera_review.json`: prefab world TRS 합성, 실제4800프레임 본 범위/frustum0, PNG120장 단일색 결과.
- 읽은 코드: `avatar_modeling/v349_remaining_npr/MacWorldWindAudit.cs`, `MacRemainingSecondaryAudit.cs`, `MacWorldWindBuildViews.cs`.
- 읽은 자산: `unity_vrchat_avatar/Assets/BiancaRemainingNpr/Bianca_v349_WorldWind.prefab`, `.controller`, `Assets/BiancaIntegration/BiancaWindOnly.mask`, `Bianca_TPoseReference.asset`.

읽기 전용 파일/API 조사만 수행했습니다. Unity bridge 호출·실행·프로젝트 편집은 하지 않았습니다.

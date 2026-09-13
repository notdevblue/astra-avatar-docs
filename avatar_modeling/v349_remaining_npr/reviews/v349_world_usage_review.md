> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v349 World Usage 후속 독립 검토

## 판정

**SDK Editor 어댑터 환경에서 World Usage ContactSender→Avatar Usage Receiver→Animator WorldWind 경로를 통과했다.** 이전 시험의 sender Usage가 Avatar였던 문제는 이번 기록에서 해소됐다. 실제 VRChat 월드/클라이언트/네트워크 검증은 아니다.

입력 없는 월드의 수동 바람을 유지하고, 호환 태그가 있는 구역에서 저작된 바람 clip을 활성화하는 방식이다. 일반 Unity WindZone이나 월드 풍향·풍속을 자동 감지하는 구현은 없다.

## 완료 증거

- `avatar_modeling/v349_remaining_npr/world_wind/world_usage/RUN.json`: error null, 4,800개 연속 frame 0…4,799.
- 전체 4,800 frame의 주 송신자 Usage는 **World**, receiver Usage는 **Avatar**.
- receiver=1은 1,680 frame, receiver=0은 3,120 frame. SDK receiver 값과 Animator WorldWind 값 차이는 전 프레임 **0**.
- 이전 Avatar Usage 시험의 `world_wind/RUN.json`은 별도로 보존됐다. 이전 실행과 비교하면 sender_usage를 제외한 기록값은 모두 정확히 같다. 즉 같은 접촉·manual 시나리오가 World flag 후속에서도 같은 결과를 냈다.
- 현재 Unity 실행 소스와 버전 폴더의 `MacWorldWindAudit.cs` SHA256은 같다. 이전 소스는 `MacWorldWindAudit_AvatarUsageTrial.cs.txt`로 남아 있다.

## 구간별 동작

다음 표는 manual `BiancaWind` 0 / 0.35 / 0.7 / 1 네 시나리오에서 모두 동일하다. 총 28개 구간에서 예상값과 다른 기록은 0개다.

| 상대 frame | 입력 | receiver / Animator WorldWind |
|---|---|---|
| 0–119 | 맞는 송신자 없음 | 0 / 0 |
| 120–359 | 맞는 태그 1개 | 1 / 1 |
| 360–479 | 맞는 태그 2개 중복 | 1 / 1 |
| 480–539 | 첫 송신자 이탈, 두 번째 잔류 | 1 / 1 |
| 540–599 | 두 번째도 이탈 | 0 / 0 |
| 600–719 | 틀린 태그만 진입 | 0 / 0 |
| 720–1199 | 송신자 없음 | 0 / 0 |

중복 송신자는 값을 2로 더하지 않고, 한 송신자가 빠져도 남은 접촉을 유지했다. 두 번째도 이탈하면 원래 수동 바람으로 돌아간다. 틀린 태그는 반응하지 않았다. manual 파라미터는 각 시나리오에서 지정한 0/0.35/0.7/1 그대로 유지됐다.

## World flag와 출력의 출처

현재 시험 코드는 다음 순서다.

1. 실제 VRCContactSender 구 3개(맞는 태그, 틀린 태그, 두 번째 맞는 태그)를 생성한다.
2. 활성화 후 Update에서 세 송신자의 Usage가 World인지 확인하고, 다르면 Editor reflection으로 설정한다. 위치만 이동하여 진입·이탈을 만든다.
3. SDK가 접촉을 판정한다. `IAnimParameterAccess` 어댑터의 setter가 실제 receiver 출력을 Animator WorldWind에 전달한다.
4. SDK `OnFrameCompleteLate` 이후 실제 receiver와 Animator 값, 본 위치·로컬 회전, 주 sender Usage를 기록한다.

SDK 3.10.5의 `VRC.Dynamics.ContactReceiver.ValidateCollider`를 IL로 대조했다. 이 메서드는 **현재** 송·수신자의 `get_Usage()`를 읽어 양쪽 `contentTypes` 허용 여부를 확인하고, `AffectedByAllowFlags`에서도 Usage를 읽는다. 따라서 현재 시험은 이름만 World로 출력한 시나리오가 아니라 해당 런타임 속성을 SDK가 사용하는 경로다. 다만 개별 SDK job 내부 상태를 추가로 덤프한 것은 아니다.

runner가 시나리오별 WorldWind를 임의로 직접 입력한 것은 아니다. 실제 SDK receiver 결과를 Animator에 쓰는 어댑터가 있으며, 이는 클라이언트의 자동 파라미터 바인딩을 대신하는 Editor 전용 연결이다. Production WorldWind.prefab에는 `MacWorldWindAudit.cs`의 GUID `23f0dfab3d9dc4af9a14eb5d1deda41c` 참조가 없음을 확인했다. 시험 runner는 별도 scene의 GameObject에 붙인다.

## 수동 바람 보존과 실제 본 반응

기존 Gentle wind 레이어 안에 nested BlendTree를 넣고, 원래 manual 트리를 WorldWind=0 자식으로 유지한다. clip 기여도의 의도는 `E = M + W × max(0, 0.7 − M)`이다. Constant receiver라 이번 W는 0/1만 발생한다.

| 수동 M | 접촉 없음 W=0 | 접촉 있음 W=1 |
|---:|---:|---:|
| 0 | 0 | 0.7 |
| 0.35 | 0.35 | 0.7 |
| 0.7 | 0.7 | 0.7 |
| 1 | 1 | 1 |

M=0의 접촉 없는 구간에서 hair_wind/tie_anchor는 초기 회전과 수치상 동일하고, 접촉 구간에는 최대 7.267618°의 실제 입력 본 회전이 있었다.

같은 유효 기여도를 의도한 M=0/W=1 대 M=0.7 비교는 최대 0.019674°, M=0.35/W=1 대 M=0.7은 0.015182° 차이다. M=0.7/1에서 W=1 구간과 10초 뒤 W=0 구간을 비교한 차이는 0.012758°/0.018294°다. 입력에 반응하고 강한 manual을 낮추지 않는 구조는 확인되지만, 이 비교는 clip 주기·다른 시나리오를 이용한 진단이다. **원래 manual-only controller와 동기화한 전 프레임 정확 일치 증명은 아니다.** 기여도 수식을 quaternion 회전이나 최종 스킨 움직임의 선형성으로 확대하지 않는다.

## 남는 한계

- 실제 월드 제작/업로드, PC VRChat 클라이언트, local/remote 네트워크 동기화, late join 및 사용자 Contacts 설정을 시험하지 않았다.
- 호환 `BiancaWorldWind` 태그의 송신자가 있어야 한다. 방향은 기존 Breeze clip에 저작된 방향이며 외부 풍향 벡터를 읽지 않는다.
- 주 sender Usage는 매 frame 기록했지만 second/wrong sender의 Usage는 raw rows에 별도 기록하지 않는다. 코드에서 세 송신자 모두 같은 설정을 적용한 사실과 구간 반응을 근거로 한다.
- unity_frame/delta_time/Animator state·normalizedTime/clip weights가 없어 이 RUN 단독으로 60Hz 동적 타이밍 정확성이나 1프레임 지연을 전수 판정할 수 없다. 설정 captureFramerate=60과 SDK 콜백 이후 캡처는 코드로 확인했다.
- WorldWind 중간값, 경계에서 미세하게 반복 진입하는 떨림, 여러 플레이어, 연속 풍속/풍향은 미검증이다.
- 이전 전체 physics 감사와 이 시험은 포즈 구동 방식이 다르므로 월드 위치를 직접 비교하여 관절·콜라이더 합격으로 묶지 않는다.

상세 수치와 SHA256: `/tmp/v349_world_usage_review.json`. 재현: `/tmp/v349_world_usage_review.py`. 프로젝트와 Unity는 수정하지 않았다.

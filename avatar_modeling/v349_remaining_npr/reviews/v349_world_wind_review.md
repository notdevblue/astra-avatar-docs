> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v349 WorldWind 독립 기술 검토

## 판정

**실제 SDK Contact→Receiver→Animator 전달은 통과했지만, World Usage 송신자로 시험됐다는 주장은 현재 증거와 맞지 않는다.** 완료 RUN의 4,800개 모든 기록에서 sender_usage와 receiver_usage가 `Avatar`다. 소스는 reflection으로 `World` 지정 의도를 갖지만, 해당 의도가 실제 기록에 반영되지 않았다. 자동 Unity WindZone 검출 시험도 아니다.

현재 승인 가능한 표현은 “호환 태그를 사용하는 실제 SDK Contact 입력과 수동 바람 합성의 Editor 시험”이다. World Usage 경로는 별도 재실행에서 런타임 값과 SDK 등록을 확인해야 한다. SDK가 값을 덮어썼는지, 실행 코드와 현재 소스가 달랐는지, 다른 초기화 경로가 관여했는지는 아직 확정하지 않았다.

## 실제 입력 및 검증 결과

`world_wind/RUN.json` error null, frame 0…4,799 연속, 4개 manual 값별 1,200 frame다. SDK `OnFrameCompleteLate` 콜백에서 `receiver.paramValue`와 Animator `WorldWind`를 읽었다.

| 시나리오 구간 | frame | 실제 receiver / Animator WorldWind | 결과 |
|---|---|---|---|
| 송신자 없음 | 0–119 | 0 / 0 | 통과 |
| 맞는 태그 송신자 1개 진입 | 120–359 | 1 / 1 | 통과 |
| 맞는 태그 송신자 2개 중복 | 360–479 | 1 / 1 | 2로 더해지지 않음 |
| 첫 송신자 이탈, 두 번째만 잔류 | 480–539 | 1 / 1 | 잔류 접촉 유지 |
| 두 번째도 이탈 | 540–599 | 0 / 0 | 해제 통과 |
| 틀린 태그만 진입 | 600–719 | 0 / 0 | 잘못된 태그 무시 |
| 이후 없음 | 720–1199 | 0 / 0 | 유지 |

위 7개 구간 × manual 0 / 0.35 / 0.7 / 1의 28개 구간 모두 예상 파라미터와 다른 기록 0개다. receiver와 Animator 파라미터 차이는 전체 4,800 frame에서 0이다. manual `BiancaWind` 자체도 각 시나리오 값으로 정확히 보존된다.

전달 출처는 다음과 같다.

1. runner가 실제 `VRCContactSender` GameObject를 receiver 근처/5m 밖으로 이동한다. `WorldWind` 값을 시나리오별로 직접 강제 입력하지 않는다.
2. SDK Contact가 receiver 값을 계산한다.
3. Editor 전용 `IAnimParameterAccess` 구현의 setter가 그 값을 `Animator.SetFloat("WorldWind", value)`에 연결한다.
4. Animator의 원래 Gentle wind 레이어 안에서 새 nested BlendTree가 바람 clip 기여도를 조합한다.

따라서 “직접 대입이 전혀 없다”도 엄밀히는 틀리다. **시험 runner가 접촉 결과를 우회하여 값을 조작하지 않았으며, 실제 SDK receiver 출력의 전달 어댑터에서 Animator 파라미터를 쓴다**가 정확하다. 실제 VRChat 클라이언트의 Animator 파라미터 바인딩/네트워크 구현을 이 어댑터가 검증하는 것은 아니다.

## 합성 구현

원래 controller를 사본으로 복사하고 원래 Gentle wind 레이어 상태의 manual BlendTree를 새 바깥 트리의 WorldWind=0 자식으로 유지한다. 바람 레이어를 추가하여 같은 본 회전을 두 번 더하는 방식은 아니다.

manual M∈[0,1], WorldWind W∈[0,1]에서 clip 기여도 의도는 `E = M + W × max(0, 0.7 − M)`다. receiver는 Constant 타입이므로 이번 실제 시험의 W는 0 또는 1뿐이다.

| M | W=0 | W=1 |
|---:|---:|---:|
| 0 | 0 | 0.7 |
| 0.35 | 0.35 | 0.7 |
| 0.7 | 0.7 | 0.7 |
| 1 | 1 | 1 |

`WorldWindFloor`는 controller 내부 float 기본값 0.7이다. 표현 파라미터에는 WorldWind만 새로 추가했고, networkSynced=true, saved=false, default=0이다. 보고된 전체 expression 비용은 16, manual 기본값은 0.35로 유지된다. 새 receiver 반경은 월드 0.02m, tag는 `BiancaWorldWind`, allowSelf=false, allowOthers=true, localOnly=false다. 시험 Sender는 반경 0.3m 구이며 잘못된 태그는 `UnrelatedWind`다.

수식은 clip 기여도 설명이다. quaternion 회전 및 물리 결과가 E에 선형 비례한다고 주장하지 않는다.

## 실제 본 회전 확인

- manual=0, 접촉 없는 구간의 hair_wind/tie_anchor 회전은 초기값 대비 수치 오차 수준(약 2e−22°), 접촉 구간은 최대 7.267618° 반응했다. 파라미터만 변하고 본이 안 움직인 시험은 아니다.
- WorldWind=1에서 M=0과 M=0.7의 같은 phase-frame 입력 회전 차이는 최대 0.019674°, M=0.35와 M=0.7은 0.015182°였다.
- M=0.7/1에서 W=1 구간과 10초 뒤 같은 clip 주기의 W=0 구간을 비교하면 최대 차이는 각각 0.012758°/0.018294°다.

이 차이는 작지만 **원래 manual-only controller와 시간·포즈를 동기화한 직접 A/B 비교는 아니다.** 실제 clip phase/정규화 시간도 기록되지 않았다. 따라서 “manual 파라미터 보존, 트리 구조상 원래 경로 보존, 본 회전 반응 확인”까지 쓰고 “원래 애니메이션 전 프레임 정확 일치”로 확대하지 않는다. 현재 avatar는 별도 world-wind runner에서 자연 Animator 포즈로 평가되므로 이전 HumanPose를 고정한 전체 physics 감사와 직접 월드 위치를 비교하지 않는다.

## 오류 및 다음 확인점

1. **World Usage 반영 실패:** 모든 sender_usage=Avatar. 실행 중 실제 Usage 및 SDK 등록 내용 확인이 필요하다. Make에서 inactive 상태에 reflection을 적용한 사실만으로 World 프로토콜 통과라 쓰지 않는다. 동일 SDK IL의 Usage getter는 `_assignedUsage` 우선, 없으면 DefaultUsage, 마지막 Avatar로 반환하므로 기록은 실제 getter 결과다.
2. **실행 메타데이터 부족:** unity_frame, delta_time, fixed_time, Animator state/normalizedTime, 실제 clip weights를 기록하지 않는다. 이 RUN만으로 60Hz 동적 정확성/지연을 전수 판정할 수 없다. 설정은 captureFramerate=60이며 callback 이후 상태라는 점은 코드로 확인된다.
3. **나머지 송신자 메타데이터 부족:** second/wrong sender의 실제 Usage·거리·활성 상태는 raw rows에 없고 코드 시나리오로만 확인한다. 같은 sender 시험 이후 합쳐 보고할 때 이 경계를 적는다.
4. **네트워크 미검증:** 로컬/원격 동기화, late join, 여러 사용자 접촉, 클라이언트 안전 설정, 아바타 Contacts 비활성 상태는 미시험이다.
5. **월드 바람의 범위:** 실제 월드가 호환 tag의 ContactSender를 배치해야 한다. 태그는 scalar 활성 신호이며 풍향·풍속을 자동 읽지 않는다. 방향/강도는 기존 Breeze clip에 저작된 움직임이다. 일반 WindZone·임의 월드 자동 반응으로 소개하지 않는다.
6. **비활성 입력 보존:** 입력이 없으면 M=.35의 기존 ambient/manual 동작이 유지되는 구조다. 다만 현재 자동 시험은 0/.35/.7/1 네 값이며 WorldWind 중간값·접촉 경계 떨림·임계 주변 출입은 미시험이다.

증거: `/tmp/v349_world_wind_review.json`에 RUN SHA256, 코드 SHA256, 구간별 실제값, 회전 비교를 기록했다. 재현 스크립트는 `/tmp/v349_world_wind_review.py`다. 프로젝트나 Unity는 변경하지 않았다.

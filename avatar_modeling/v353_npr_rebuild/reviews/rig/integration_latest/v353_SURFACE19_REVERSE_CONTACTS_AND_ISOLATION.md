> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../../../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v353 역순 실제 접촉과 공정한 실행 분리

역순 실제 표면을 캐시 없이 새로 계산했다. **PAD1을 먼저 실행해도 넥타이–셔츠 교차는 21표본 모두 0이고, 두 번째 BASE는 frame90에서 26쌍이 남는다.** 넥타이 후보의 확인된 접촉 개선은 단순 실행 순서 효과로 사라지지 않았다.

| 역순 조건 | 넥타이–셔츠 | 넥타이–코트 | 코트–바지 |
|---|---|---|---|
| 첫 번째 PAD1 | 0/21표본 | 0/21 | 0/21 |
| 두 번째 BASE | frame90에서 26쌍, 나머지0 | 0/21 | 0/21 |

BASE frame90 최대 교차선 길이는 **10.6472361mm**다. 최초 첫 번째 BASE의 10.6527243mm와 작은 차이가 있으나 26쌍이라는 접촉 문제는 유지된다. 이 길이는 관통 깊이가 아니라 두 삼각형의 교차선 길이다.

현재 source19 BodyHair의 100개 제거 삼각형은 이전 눈 정리 기록의 정확한 unity_indices와 맞췄으며 헤어 제거로 오인하지 않았다. 남은 삼각형의 방향·원형 대응, Coat 원형 삼각형도 재검증했다. 실제 source 두 메시, 84개 입력 binary, 제거기록, 원형 source/map/meta 6개, endpoint/현재분석 코드, RUN/DONE/장면/실행지문까지 **101개 SHA**를 기록했다.

검사 범위는 각21시점, 공유 정점의 인접면 제외, 비공면 교차선 **0.05mm 이상**이다. 이 자료에서는 공면 미결 후보 수0이다. 600프레임 전수·연속 충돌·전체 자기교차·PC클라이언트 합격으로 확대하지 않는다.

## 이후 실행기의 순서 의존성 제거 방안

현재 실측으로 확인한 문제는 **한 Play 세션에서 두 번째 아바타를 만드는 경우의 결정적 차이**다. `StartNext()`는 이전 인스턴스를 비활성화하고 새 인스턴스를 생성하지만, Play 세션 자체는 이어진다. SDK 내부 카운터/등록/초기화 중 무엇이 원인인지는 아직 확인되지 않았다. 불확실한 SDK 내부 상태를 수동 초기화하거나 warmup 숫자만 늘리는 방안은 첫 수정으로 권하지 않는다.

가장 직접적인 수정은 **후보마다 별도 Play 세션에서 첫 번째 인스턴스로 실행**하는 것이다.

1. BASE 하나만 기존 actual harness로 실행하고, DONE뿐 아니라 EnteredEditMode의 SCENE_PRESERVATION equal까지 기다린다.
2. 새 Play 세션에서 PAD1 하나만 동일 harness·source·입력·90warmup으로 실행한다.
3. 둘 다 ordinal=0이라는 실행 메타데이터, 코드/입력/source SHA, 같은 입력 프레임 기준을 저장한다. 이후 첫 번째 결과끼리 비교한다.

기존 진입점을 사용한다면 각각 별도 호출이다. 실행 자체는 부모가 담당한다.

```csharp
MacV353PhysicsAuditViews.BeginBatch("SURFACE19_TIE_BASE", "tie_detail", "surface19_tie_isolated_base");
// BASE Play 종료 + 장면 복원 완료 확인 후 다음 호출
MacV353PhysicsAuditViews.BeginBatch("SURFACE19_TIE_PAD1", "tie_detail", "surface19_tie_isolated_pad1");
```

현재 읽은 `ProjectSettings/EditorSettings.asset`는 `m_EnterPlayModeOptionsEnabled: 0`이므로 통상적인 domain/scene reload 설정이다. 저장된 Options 비트3만 보고 reload 비활성이라고 해석하지 않는다. 프로젝트 설정을 임의로 변경하지 않고 실행 시 해당 상태를 기록·확인한다. Unity 문서에 따르면 domain reload는 static 필드와 등록 핸들러를 초기화한다. 단, 이것만으로 SDK native 내부 상태 전체 초기화가 증명되는 것은 아니므로 실제 같은 조건 반복의 본/표면 관문은 유지한다. [Unity 2022.3 Domain Reloading](https://docs.unity3d.com/2022.3/Documentation/Manual/DomainReloading.html), [Enter Play Mode 설정](https://docs.unity3d.com/2022.3/Documentation/Manual/ConfigurableEnterPlayMode.html)

이 변경은 후보 메시·본·웨이트·물리값을 조절하는 수정이 아니라 검증 실행기를 공정하게 분리하는 수정이다. SDK의 실제 내부 원인까지 찾고 싶다면 이후 독립 계측으로 진행하되, 검수 사진과 수치는 먼저 같은 첫 실행끼리 비교한다.

재현 코드 (공유본 미포함: `check_surface19_reverse_contacts_v1.py`) · 새 접촉 결과와 전체 지문 (공유본 미포함: `v353_SURFACE19_REVERSE_CONTACTS_V1.json`) · [역순 본·표면 대조](v353_SURFACE19_REVERSE_REVIEW.md)

Assets 및 Unity/Blender UI는 변경하지 않았다.

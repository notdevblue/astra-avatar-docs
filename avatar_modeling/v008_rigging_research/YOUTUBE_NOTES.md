> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# YouTube 관절 제작 강의 조사 노트

**2026-09-09 · 제작자 원본 8편 · PC Unity / VRChat 적용 관점**

각 영상의 원본 자막에서 필요한 구간을 읽고 공식 문서와 대조했다. 팔꿈치 영상은 재생 장면도 표본 확인했다. 전체 영상 8편을 실시간으로 모두 완주했다는 뜻은 아니다. 자동 자막은 전문 용어가 틀리는 경우가 있어 원문 표현을 그대로 규칙으로 옮기지 않았다. 원본 자막 파일은 임시 열람 자료이며 저장소에 포함하지 않는다.

표의 구간은 실제로 읽은 관련 자막의 범위다. 링크의 `t=`는 다시 확인하기 편한 시작점이며, 전체 구간을 재생했다는 표시가 아니다. 게시일을 직접 확인하지 못한 영상은 추정 날짜를 쓰지 않았다. 제작자별 결과가 서로 다른 캐릭터·리그·모디파이어를 사용한다는 점을 고려한다.

## 1. CGDive — RIGGING L3-6 : Pro Weight Painting

[영상 열기](https://www.youtube.com/watch?v=rf3NDKXYpTk) · 약 54분 · 확인한 게시일 2025-04-15 · 자동 영어 자막 · [출처 23](SOURCES.md#source-23)

| 읽은 구간 / 다시 볼 시작점 | 관찰한 핵심 |
|---|---|
| [13:04 무릎 부근](https://www.youtube.com/watch?v=rf3NDKXYpTk&t=784s) | 포즈를 만들어 웨이트의 실제 결과를 확인하는 흐름 |
| [37:42–43:09 머리·목](https://www.youtube.com/watch?v=rf3NDKXYpTk&t=2262s) | 머리의 안정적인 추종, 불필요한 그룹 영향 정리, 개별 정점 검사 |
| [43:09–49:33 목·칼라](https://www.youtube.com/watch?v=rf3NDKXYpTk&t=2589s) | 목과 주변 의상의 연결을 같이 보며 부위별 웨이트 조정 |

현재 모델에 가져올 것은 얼굴·목·칼라를 역할별로 구분하고 같은 자세에서 함께 검사하는 순서다. 강의 예시의 0.5/0.5, 0.75/0.25 같은 비율은 그 캐릭터의 예시이며 우리 모델의 정답으로 복사하지 않는다. 전체 웨이트 삭제나 오브젝트 Join을 무조건 따라 하지 않는다. Join은 모든 경계 정점을 Weld하는 것과도 다르다.

이 강의의 가장 중요한 적용점은 목을 높이 하나로 분류하는 기존 규칙을 재검토하는 것이다. 구체적인 변경 범위는 [목 작업 계획](IMPLEMENTATION_PLAN.md#4-단계-1--목과-칼라)에 연결한다.

## 2. CGDive — RIGGING L2-2 : IK, Fingers, Bone Roll

[영상 열기](https://www.youtube.com/watch?v=QnCo0hzXKeQ) · 약 16분 · 자동 영어 자막 · [출처 24](SOURCES.md#source-24)

**확인 구간:** [00:00–04:30](https://www.youtube.com/watch?v=QnCo0hzXKeQ&t=1s), 손가락 본 위치·축·엄지의 다른 방향을 다루는 부분.

손가락 뿌리의 중심을 피부 사이의 패인 선에만 맞추지 않고 손바닥 안쪽 관절 위치와 함께 봐야 한다는 점, 손가락마다 굽힘 방향을 확인해야 한다는 점을 채택한다. “옆으로 굽히지 않는다”는 자막을 손가락 전체에 적용하지 않는다. 손바닥 관절의 벌림과 중간/끝 마디의 주된 굽힘을 구분해 해석한다.

우리 모델은 손 형상은 있지만 개별 손가락 본이 없다. 이 영상은 새 손을 만드는 근거가 아니라 기존 손 내부에 본을 배치하고 양손 제스처로 검증하는 순서의 근거로 사용한다.

## 3. Anthony Gibbs — 3D Character Creation for Animation - The Shoulder

[영상 열기](https://www.youtube.com/watch?v=c_XM01I6ed0) · 약 6분 · 제공 영어 자막 전체 읽음 · [출처 25](SOURCES.md#source-25)

| 구간 | 확인 내용 |
|---|---|
| [00:34](https://www.youtube.com/watch?v=c_XM01I6ed0&t=34s) | 어깨 둘레의 면 흐름과 다방향 움직임 |
| [01:26](https://www.youtube.com/watch?v=c_XM01I6ed0&t=86s) | 회전 중심 배치의 중요성 |
| [02:42](https://www.youtube.com/watch?v=c_XM01I6ed0&t=162s) | 가슴/등 쪽 보조 변형 아이디어 |
| [03:38–끝](https://www.youtube.com/watch?v=c_XM01I6ed0&t=218s) | Bendy Bones, Preserve Volume, 국소 Corrective Smooth |

우리 모델은 쇄골을 고정하고 상완만 들어 올린 기존 시험의 한계를 먼저 수정해야 한다. 강의의 어깨를 새로 만드는 면 구조를 그대로 복제하는 것은 사용자의 작은 수정 조건과 맞지 않는다. Bendy Bones와 후처리 모디파이어의 좋은 결과는 Blender 내부 사례로 기록하고, PC에서는 일반 변형 본이나 별도 검증한 Constraint 경로로 가능한지 확인한다.

## 4. Anthony Gibbs — 3D Character Creation for Animation - Elbow Topology

[영상 열기](https://www.youtube.com/watch?v=dEvWZwRxeWQ) · 약 4분 39초 · 제공 영어 자막 전체 읽음 · 재생 장면 표본 확인 · [출처 26](SOURCES.md#source-26)

| 구간 | 확인 내용 |
|---|---|
| [00:30–01:17](https://www.youtube.com/watch?v=dEvWZwRxeWQ&t=30s) | 한 줄 관절과 지지선, 안쪽 압축/바깥쪽 늘어남의 차이 |
| [01:17–02:16](https://www.youtube.com/watch?v=dEvWZwRxeWQ&t=77s) | 면 간격과 윤곽 지지. 이 구간의 실제 재생 장면도 표본 확인 |
| [03:09–끝](https://www.youtube.com/watch?v=dEvWZwRxeWQ&t=189s) | 같은 토폴로지라도 스키닝/후처리에 따라 다른 결과 |

무릎과 팔꿈치의 바깥 윤곽·안쪽 접힘을 별도로 평가하는 원리를 가져온다. “관절에는 무조건 세 루프” 또는 “삼각형을 없애면 해결”이라는 규칙은 채택하지 않는다. Preserve Volume으로 생기는 둥근 부풀음과 Corrective Smooth 결과는 기본 LBS와 따로 비교해야 한다.

## 5. Dikko — Modeling For Animation 01 - TOP 5 Features of Good Animation Models!

[영상 열기](https://www.youtube.com/watch?v=01WQlMD7dsk) · 약 30분 · 자동 영어 자막 · [출처 27](SOURCES.md#source-27)

**읽은 주요 구간:** 약 09:00–19:45. [13:04 손·엄지](https://www.youtube.com/watch?v=01WQlMD7dsk&t=784s), [15:35 압축과 늘어남](https://www.youtube.com/watch?v=01WQlMD7dsk&t=935s), [18:13 손가락 관절](https://www.youtube.com/watch?v=01WQlMD7dsk&t=1093s).

면 흐름은 모양을 예쁘게 나누는 목적 외에 실제 포즈에서 압축·늘어남과 주름을 표현하는 역할이 있다. 손바닥의 엄지 밑 패드와 관절 바깥 윤곽의 지지, 몸통·고관절의 흐름을 구분해 확인했다. 균일하게 많은 루프를 넣어도 원하는 관절 윤곽이 자동으로 생기는 것은 아니라는 관찰이 중요하다.

이 영상은 처음부터 모델을 설계하는 맥락이다. 우리 작업에서는 기존 형상의 부족한 국소 부위를 판정하는 원리만 가져오고, 전체 대칭 재구성·광범위한 폴 정리·캐릭터 재모델링은 실행하지 않는다.

## 6. Pierrick Picaut — Rigging hanging equipment in Blender

[영상 열기](https://www.youtube.com/watch?v=Vg2oS1P9m2o) · 제공 영어 자막 · [출처 28](SOURCES.md#source-28)

**읽은 구간:** [00:00–05:17](https://www.youtube.com/watch?v=Vg2oS1P9m2o&t=1s), 몸에 매단 물체의 회전 추종과 별도의 기울기 제어를 설명하는 부분.

몸이 방향을 바꾸는 움직임과 물체가 중력 방향으로 매달려 보이는 움직임을 나눠 생각하는 방식을 확인했다. 넥타이 매듭과 아래 부분, 자락 뿌리와 끝의 역할을 설계할 때 참고한다. 영상의 축 이름을 Unity 좌표로 그대로 치환하지 않고 부모/월드 공간을 따로 기록해야 한다.

이 방식은 실제 천의 모든 충돌을 계산하는 물리가 아니다. PC에서는 고정 스키닝·옷 본·PhysBone 중 어떤 역할로 구현할지 다시 분류한다.

## 7. CGDive — RIGGING L3-3 : Improved Leg Rig

[영상 열기](https://www.youtube.com/watch?v=ErR4RysRy0g) · 약 59분 · 자동 영어 자막 · [출처 29](SOURCES.md#source-29)

**읽은 구간:** 00:00–02:40의 도입과 [03:43–09:40의 발 피벗 구성](https://www.youtube.com/watch?v=ErR4RysRy0g&t=223s).

발끝·뒤꿈치·발볼·안쪽/바깥쪽의 기준점을 나누면 발의 굴림을 명확히 제어할 수 있다. 이 피벗들은 제작용 IK 제어 장치이며 피부를 직접 변형하는 발/발가락 본과 역할이 다르다. 자막의 “dump truck”은 문맥상 Damped Track을 가리키는 오기로 보이므로 그대로 기술 명칭으로 사용하지 않는다.

현재 구두는 굽과 밑창이 단단해야 한다. 발가락 본 후보를 검토하되 강의의 전체 제어 계층을 최종 VRChat에 그대로 넣거나 보행 IK를 대체하지 않는다.

## 8. Pierrick Picaut — This Blender Trick Makes Weight Painting EASY

[영상 열기](https://www.youtube.com/watch?v=4VI5jk7pZag) · 약 12분 · 자동 영어 자막 · [출처 30](SOURCES.md#source-30)

**확인:** 앞부분의 웨이트 블록/블러 설명을 발췌 확인하고, [08:19–11:30](https://www.youtube.com/watch?v=4VI5jk7pZag&t=499s)의 변형 시험·웨이트 전송 설명을 읽었다.

단순한 보조 표면에서 변형을 설계하고 상세 모델에 웨이트를 옮기는 방법이다. Surface Deform으로 시험한 결과와 Data Transfer로 실제 웨이트를 옮긴 결과가 완전히 같지는 않다고 설명한다. 최종 예시에서는 Vertex Groups와 Nearest Face Interpolated 전송을 사용한다.

원본 상세 메시를 새로 만드는 방식과는 구분되지만, 현재 작업의 기본 경로로 채택하지 않는다. 보조 표면 제작·전송 오차·서로 가까운 의상 표면의 잘못된 대응이 추가되기 때문이다. 기존 모델의 직접 국소 웨이트 작업이 막힐 경우에만 별도 후보로 설명할 수 있으며, Surface Deform 자체를 PC 런타임 기능으로 약속하지 않는다.

## 강의들을 함께 읽었을 때의 적용 원칙

본 위치와 축을 먼저 확인하고, 실제 포즈에서 웨이트를 조절하며, 필요한 접힘 공간이 부족할 때만 국소 토폴로지를 검토한다. 강의에서 잘 된 화면을 보고 그 모디파이어·리그 전체를 복사하지 않는다. 현재 캐릭터의 원형 보존과 실제 PC 런타임 동작을 각각 통과해야 한다.

이 노트는 영상의 대체 자막이 아니라 프로젝트 적용 판단 기록이다. 제작자의 전체 설명과 예시는 연결된 원본 영상에서 확인할 수 있다.

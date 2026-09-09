> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v014 추가 조사 — 관절 보정과 엔진 검증의 구분

2026-09-09. 사용자가 남은 문제를 수정하고 모르면 자료를 조사하라고 요청했다. 아래는 이번에 검색/열람한 공식 자료와 실제 작업에 적용한 판단이다. 새 유튜브 영상을 시청했다고 주장하지 않는다. 이전 영상 조사 기록은 v008/v012에 보존되어 있다.

## 1. 웨이트와 부피 손실

[Blender 5.2 Armature Modifier](https://docs.blender.org/manual/en/latest/modeling/modifiers/deform/armature.html)는 일반 관절 혼합에서 회전이 커질수록 주변 부피가 줄 수 있으며, Preserve Volume은 quaternion 기반의 다른 계산을 사용한다고 설명한다. 이번에는 이 옵션을 켜서 Blender에서만 모양을 좋게 만드는 변경을 하지 않았다. 같은 선형 변형 조건에서 국소 웨이트 후보를 비교했다.

단순한 두 회전의 50:50 선형 혼합에서 90도 굽힘의 중간 방향 길이는 원래의 약0.707배가 될 수 있다. 이는 원인을 설명하는 수학적 예이며 현재 무릎이 정확히 그 비율로 줄었다는 실측이 아니다. 웨이트를 잘 분배해도 별도 변형 자유도 없이 모든 부피 손실을 없앤다고 보장할 수 없다. 보조 본 제안은 이 한계를 다루는 다음 실험이며 아직 구현하지 않았다.

[CMU 연구 목록](https://graphics.cs.cmu.edu/publications/)과 *Real-time Skeletal Skinning with Optimized Centers of Rotation*의 검색 결과도 확인했다. 개별 연구 소개 페이지는502로 열리지 않았고 논문 전체를 검토하지 않았다. 따라서 그 알고리즘을 이번 모델에 구현했다거나 채택했다고 쓰지 않는다.

## 2. Unity의 실제 관절 범위 검사

[Unity 2022.3 Avatar Muscle & Settings](https://docs.unity3d.com/2022.3/Documentation/Manual/MuscleDefinitions.html)는 본 매핑 후 근육 그룹/개별 범위를 미리 보며 변형을 검사하는 절차를 설명한다. Humanoid 유효성은 매핑을 사용할 수 있는지에 관한 조건이며 실제 겨드랑이·무릎 외형의 합격 판정이 아니다. v014에서는 기존 몸 본 이름19개를 Humanoid 슬롯에 명시적으로 매핑하고 T-pose 참조를 설정하는 Editor 코드를 준비했다. 원래 root는 Humanoid 관절 슬롯에 넣지 않는다.

[VRChat Rig Requirements](https://creators.vrchat.com/avatars/rig-requirements/)는 페이지 자체가 오래된 내용이라고 경고한다. SDK2의 손가락·UpperChest 경고를 SDK3의 현재 필수 조건으로 잘못 옮기지 않았다. 기본 계층과 실제 SDK 검증 결과를 함께 확인한다. 손가락 없는 현재 모델을 손가락 구현 완료로 취급하지 않는다.

## 3. 자락 추종과 충돌은 별개

[VRChat PhysBones](https://creators.vrchat.com/common-components/physbones/)는 본 체인의 보조 움직임과 충돌체를 다루며 의상에도 활용할 수 있다. 모든 천 표면을 검사하는 삼각형 단위 cloth 해결책이라는 뜻은 아니다. 그래서 자락을 몸통에 더 따라가게 했다는 이유만으로 소매와 바지 모두 안전하다고 판단하지 않고 두 교차 지표를 같이 검사했다.

실제로 v014의 강한 몸통 추종은 소매 접촉을0으로 만들면서 바지 교차를194/204쌍으로 늘렸다. 이 조합은 제외했다. 앞/뒤 자락을 여는36조합도 남은 접촉을 충분히 없애지 못해 채택하지 않았다. 콜라이더 크기만 과하게 키워 외형을 띄우는 해결도 채택하지 않았다.

## 4. 기존 본을 유지하는 보조 변형 구조

[VRChat Rotation Constraint](https://creators.vrchat.com/common-components/constraints/vrc-rotation-constraint/)는 여러 소스의 회전을 가중하여 대상 회전을 정할 수 있고, 중립 offset을 유지하며 활성화할 수 있다. [공식 Constraints API](https://creators.vrchat.com/common-components/constraints/constraints-api/)는 소스 설정, 활성화, 구성 변경 적용 방법을 제공한다. 이는 SDK Editor 도구에서 구성하기 위한 근거이며, 임의 런타임 C#이 아바타에서 실행된다는 뜻이 아니다.

이를 이용한 무릎/겨드랑이 중간 회전 보조 본4개를 `NEXT_SCOPE.md`에 제안했다. 사용자 요구에 따라 큰 구조 변경 전에 별도로 확인하며, 현재32본 모델에는 추가하지 않았다. 실제 회전 결과와 원형/웨이트·성능 검사까지 통과해야 후속 후보가 될 수 있다.

## 5. 현재 실행 환경과 버전

[VRChat 지원 Unity 버전](https://creators.vrchat.com/sdk/upgrade/current-unity-version/)은 이번 조회에서도 **2022.3.22f1**이다. [Unity 공식 릴리스](https://unity.com/releases/editor/whats-new/2022.3.22f1)의 Windows 설치 파일을 내려받아 Unity Technologies의 유효한 디지털 서명을 확인했다. 별도 설치 시 Windows 관리자 승인(UAC)에서 대기했으며 사용자에게 필요한 조작을 알렸다. 승인 전에는 설치 완료로 기록하지 않는다.

기존 설치2022.3.62f2는 새 빈 프로젝트 생성/정상 종료에 성공했다. 따라서 과거 기록의 “라이선스 때문에 시작 불가”를 현재 장애로 사용하지 않는다. 62f2의 별도 probe는 코드·가져오기 확인용이며 공식 지원 버전의 VRChat Build & Test와 구분한다.

[SDK 3.10.5 공식 릴리스](https://creators.vrchat.com/releases/release-3-10-5/)는2026-09-04 발표다. 공식 VPM index에서 base/avatars3.10.5의 URL과 SHA-256을 읽고, 내려받은 ZIP의 해시를 대조해 별도 프로젝트를 준비했다. 설치 캐시는 작업 Git/공유 문서 업로드에서 제외한다. 현재 환경에서 필요한 Android JNI 컴파일 모듈은 SDK의 Oculus 의존성 때문에 포함했으며, Android/Quest 빌드 대상으로 바꾸거나 Quest 최적화를 한 것이 아니다.

## 6. 가져오기 표면 검증

[Unity BakeMesh API](https://docs.unity3d.com/2022.3/Documentation/ScriptReference/SkinnedMeshRenderer.BakeMesh.html)의 useScale와 renderer 상대 좌표 설명을 확인했다. 검사 도구에서 스케일 처리를 잘못하면100배 좌표 오류가 생겼다. 명시적 BakeMesh(mesh,true) 뒤 renderer.localToWorldMatrix로 옮겨 원본과 비교했다. 이 오류는 검증 코드의 좌표계 오류였으며 실제 렌더의 모델 크기가100배였다는 뜻이 아니다. 이어 quad 대각선 차이를 찾아 FBX에 원래 loop triangle을 명시했고 삼각형/UV 검사를 통과했다.

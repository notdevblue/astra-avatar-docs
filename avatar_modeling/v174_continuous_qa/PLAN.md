> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# 연속 Humanoid·표정 외 손가락·머리·옷 물리 검증

v170 센서 공간 분리 후192정적자세의 원점 이동/회전/스케일 검사를 통과했다. 정적 통과를 연속 동작 통과로 확대 해석하지 않는다.

실제 SDK Contacts→Animator24키,30제약,11PhysBones,바람 레이어를 켠 상태에서 Unity Humanoid IK 손발 목표를 연속 변경한다. SDK idle 클립의 근육 채널 형식에 따라 머리·목·몸통과 손가락 펼침/주먹 채널을 추가한 **검사용** 클립을 사용한다. 바람/물리 ON과 OFF 각각1200프레임, 정지→동작→정지 복귀, 루트 이동을 포함한다.

검증: 모든 좌표 finite, 뼈 길이와 키0–1범위, 직접 계산한 회전 기반 기대키와 실제키의 프레임별 차이·최적 지연, 표면 면적비/교차와 다각도 사진. Contacts 갱신은 Animator와 같은 시점이 아닐 수 있으므로 정지 오차와 연속 지연을 나누어 보고한다. 아무 런타임 테스트 C#도 납품 아바타에 저장하지 않는다.

원시 메시·사진은 `.venv/bianca_continuous`에만 저장한다. 실제 VRChat 클라이언트는 실행하지 않는다. 참고: [Unity OnAnimatorIK](https://docs.unity3d.com/2022.3/Documentation/ScriptReference/MonoBehaviour.OnAnimatorIK.html), [VRChat Contacts](https://creators.vrchat.com/common-components/contacts/), 설치 SDK의 `proxy_afk.anim` 손가락 근육 바인딩(`LeftHand.Index.1 Stretched`).

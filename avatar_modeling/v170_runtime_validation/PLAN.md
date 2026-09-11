> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# Unity 통합 검증 계획

실제 VRChat 클라이언트, 업로드, Build & Test는 실행하지 않는다. v167 SDK 3.10.5 Editor Play Mode 검증을 확장한다.

1. Cuff 65% 회전의 Blender/SDK 차이를 회전 구간 분할로 줄이고 32자세 좌표 대조를 반복한다.
2. 자동 FX에 바람 레이어/마스크가 추가된 프리팹에서 이동·회전·균일 스케일 변경 후 정적 보정값을 대조한다. SDK Contacts→Animator 계산은 실제 실행하고 키 직접 대입은 하지 않는다.
3. Humanoid Animator의 IK Pass + OnAnimatorIK로 팔 앞/뒤/위/가로, 양 다리 목표를 지정한다. 바람·PhysBone·자동 보정을 동시에 켜고 목표 도달 오차, 뼈 길이, finite 좌표, 키 범위, 표면 변형과 다각도 사진을 기록한다. 수동 본 회전과 Humanoid IK 결과를 혼동하지 않는다.
4. 94 Contacts, Animator 트리와 물리 비용을 별도 명시한다. Local Only로 원격 동작을 없애 성능 등급만 숨기는 해결은 채택하지 않는다. 실제 SDK 성능 검사/API와 Editor 측정의 한계를 기록한다.
5. 관절/옷의 남은 형태 이슈는 Blender 별도 후보에서 계속 개선한다. 수치 통과만으로 홈·음영·실루엣이 모두 정상이라고 판단하지 않는다.

## 확인한 근거

- [Unity 2022.3 OnAnimatorIK](https://docs.unity3d.com/2022.3/Documentation/ScriptReference/MonoBehaviour.OnAnimatorIK.html): 내부 IK 계산 직전에 목표/가중치를 설정한다.
- [Unity Inverse Kinematics](https://docs.unity3d.com/2022.3/Documentation/Manual/InverseKinematics.html): Humanoid Avatar와 Animator 레이어 IK Pass가 필요하다. 이 검사는 Unity IK이며 VRChat 자체 추적/IK와 동일하다는 주장은 하지 않는다.
- [VRChat Performance Ranks](https://creators.vrchat.com/avatars/avatar-performance-ranking-system/): PC Contacts Poor 상한32; 현재94개는 Contacts 항목 Very Poor에 해당한다. 정적 등급은 실제 Animator CPU 비용을 모두 설명하지 않는다.
- [VRChat Contacts](https://creators.vrchat.com/common-components/contacts/): Local Only는 로컬에 한정한다. 자동 자세 보정은 원격 관찰자에게도 필요하므로 단순한 등급 우회에 쓰지 않는다.

실험용 MonoBehaviour는 Unity 로컬 검사용이며 최종 아바타 프리팹에 저장하지 않는다. 출력 JSON은 완료 시 새 파일에 한 번만 기록하여 이전 반복 덮어쓰기의 IO1224 재발을 피한다.

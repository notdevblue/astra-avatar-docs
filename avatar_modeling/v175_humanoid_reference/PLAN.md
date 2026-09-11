> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# Humanoid 기준 자세 교정

연속 검사 사진의 시작부터 팔이 등 뒤로 모였다. 기존 Import 설정은 아래로 내린 원래 모델 rest를 `HumanDescription.skeleton`에 그대로 넣었다. `Avatar.isValid`와 IK 목표 도달만으로 정상적인 리타게팅을 보장하지 않는다.

[Unity 2022.3 Avatar 설정](https://docs.unity3d.com/2022.3/Documentation/Manual/class-Avatar.html)은 매핑 이후 T-pose 확인을 요구한다. [Unity 2022.3 AvatarSetupTool 원본](https://github.com/Unity-Technologies/UnityCsReference/blob/2022.3/Editor/Mono/Inspector/Avatar/AvatarSetupTool.cs)의 실제 Enforce T-Pose 경로와 설치 Editor API를 확인한다.

원래 FBX, mesh/bindpose/정점/웨이트는 그대로 두고 임시 인스턴스에서 기준 T-pose를 계산한 별도 Avatar 에셋을 만든다. 별도 프리팹에 Avatar만 교체한다. 원래 인스턴스의 본 rest는 바꾸지 않는다. 교정 전후 SDK idle/근육 기본값/실제 IK와 자동 보정, 다각도 사진을 비교한다. 실패 시 원래 파일로 되돌릴 수 있다. 새로운 메시 생성은 없다.

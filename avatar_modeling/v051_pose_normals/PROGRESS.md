> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# 자세 보정 형상과 평가 노멀의 분리

옷깃/어깨의 원형 주름을 무조건 펴는 방식은 채택하지 않는다. 앞선 자세키 사본의 함몰 일부는 코트 raw정점 연결과 평가 노멀을 재계산한 진단에서 달라졌다. 현재는 같은 자세키/같은 좌표에 평가 후 노멀 처리만 달리하여 실제 표면 접힘과 음영을 구분한다. Weighted Normal과 Weld의 진단 결과를 비교하며 원본 사본은 저장하지 않는다.

게임으로 옮길 경우 Normals와 Blend Shape Normals의 Import/Calculate 선택을 맞추고 실제 가져온 결과를 확인해야 한다. Unity는 두 값을 맞추라고 안내하며 blend shape의 위치·노멀·탄젠트 변화를 지원한다. Blender modifier가 자동으로 실시간 실행된다는 뜻은 아니다. [Unity 공식 가져오기 문서](https://docs.unity3d.com/2022.3/Documentation/Manual/FBXImporter-Model.html).

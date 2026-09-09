> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# 옷의 변형 보정 구현 비교

어깨 보조본을 넓게 쓰는 v047은 작은 검사30표본에서도 기존 면적 경고113건보다 증가했고, 옷깃/상완의 판 같은 접힘이 커져 통합하지 않았다. 손에서 효과가 있었던 방식을 어깨에도 그대로 채택하지 않는다.

이번은 Blender의 실제 Corrective Smooth를 기존 코트 연결 사본에서 시험한다. 변형 전 표면을 BIND한 뒤 옷깃을 보호하는 정점 그룹 안에서 작동시킨다. 원래 주름을 보존하는 보정 모드이고 단순히 모든 정점을 매끈하게 펴는 방식과 다르다. 새 메시·리메시 없이 기존 정점의 평가 위치만 바뀐다. 각 설정의 중립/팔 옆올림/앞올림·앞뒤를 렌더해 비교한다.

이 modifier와 Blender driver는 Unity/VRChat에 그대로 옮겨지는 기능이 아니다. 좋은 결과가 나오면 자세 목표로 굳혀 기존 메시의 blend shape 또는 호환 보조본 구성으로 옮기고, 그 결과를 다시 검증해야 한다. modifier 렌더를 게임용 완성본으로 주장하지 않는다. [Blender Corrective Smooth 공식 설명](https://docs.blender.org/manual/en/5.0/modeling/modifiers/deform/corrective_smooth.html), [Unity 모델·Blend Shape 노멀 가져오기](https://docs.unity3d.com/2022.3/Documentation/Manual/FBXImporter-Model.html).

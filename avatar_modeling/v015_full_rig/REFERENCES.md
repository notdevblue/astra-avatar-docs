> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# 전신 본 작업의 참고 자료

2026-09-09. 원래 외형 기준 avator_references/ref_char.png를 실제 열어 확인했다. 새 캐릭터나 새 메시를 만드는 참고가 아니라 실루엣/의상/헤어 보존 기준이다.

- [OpenStax — Types of Body Movements](https://openstax.org/books/anatomy-and-physiology-2e/pages/9-5-types-of-body-movements): 이번에 본문/도해 설명을 읽었다. 굽힘, 벌림, 축 회전, 전완 회내/회외, 엄지 맞섬을 검사 항목에 반영한다. 의료 ROM 수치를 모델의 합격 기준으로 단정하지 않는다.
- [Blender Rigify 배치 지침의 구버전 공식 문서](https://docs.blender.org/manual/en/2.81/addons/rigging/rigify.html): 이번 검색 본문에서 관절 중심, 손가락 마디 위치, 축/엄지의 별도 정렬 원리를 확인했다. 최신 배치 페이지는402로 직접 열리지 않았다. 구버전 UI를 최신 절차로 복사하거나 Rigify로 전체 리그/메시를 새 생성하지 않는다.
- [Blender Copy Rotation API](https://docs.blender.org/api/4.0/bpy.types.CopyRotationConstraint.html): 로컬 공간/영향도 설정의 근거. 최신 Manual 직접 열람은402여서 실제 설치된 Blender5.2 속성과 평가 행렬로 함께 확인한다.
- [CMU — Real-time skeletal skinning with optimized centers of rotation](https://graphics.cs.cmu.edu/?p=1436): 이번 검색 결과에서 초록을 읽었다. 선형/이중쿼터니언 혼합의 관절 부피·비틀림 문제를 구분하는 참고다. 직접 페이지는 시간 초과했고 논문 전체 알고리즘을 구현하지 않았다.
- [VRChat PhysBones](https://creators.vrchat.com/common-components/physbones/): 이번 공식 문서를 다시 확인했다. 고정할 뿌리와 흔들릴 본 체인을 분리하고, 같은 오브젝트에 Constraint/PhysBone을 중복 적용하지 않는 구조를 준비한다. 실제 물리/바람 구현 완료가 아니다.

기존 제작자 영상 조사 노트도 재사용한다: [CGDive 손가락/본 축](https://www.youtube.com/watch?v=QnCo0hzXKeQ), [Anthony Gibbs 어깨](https://www.youtube.com/watch?v=c_XM01I6ed0), [팔꿈치 토폴로지](https://www.youtube.com/watch?v=dEvWZwRxeWQ). 이번 턴에 이 영상들을 새로 시청한 것으로 표현하지 않는다. 이전 자막·재생 표본의 열람 범위는 ../v008_rigging_research/YOUTUBE_NOTES.md에 있다.

적용 원칙은 회전 중심과 본 축부터 확인하고, 웨이트와 국소 면 구조의 원인을 분리하는 것이다. 다른 캐릭터 강의의 수치/전체 재모델링/Blender 전용 보정 결과를 현재 PC 아바타의 정답으로 복사하지 않는다.

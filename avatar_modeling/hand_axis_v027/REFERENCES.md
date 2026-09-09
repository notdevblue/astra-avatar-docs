> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# 손가락 축과 런타임 검수 참고

2026-09-09. 이번 조사에서 읽은 범위와 실제 모델 시험을 구분한다.

- [Unity 2022.3 Avatar Muscle & Settings](https://docs.unity3d.com/2022.3/Documentation/Manual/MuscleDefinitions.html): 공식 본문 열람. Avatar 구성 후 여러 본의 그룹 미리보기와 개별 muscle 범위를 조정해 겹침/변형을 점검한다. 따라서 Blender local X 각도 검사가 Unity의 실제 muscle/제스처 검사와 같다고 가정하지 않는다. 이번에는 Unity를 실행하지 않았으며 가동 범위를 줄여 Blender의 실패를 통과시키지도 않았다.
- [Blender Rigify 2.81 문서](https://docs.blender.org/manual/en/2.81/addons/rigging/rigify.html): 검색 결과에서 손가락 본 roll/회전축 안내를 확인했지만 원문 재열람은 오류였다. 5.2 bone-positioning/limbs 문서도 접근 오류였으므로 최신 본문을 검증했다고 기록하지 않는다. 이번 작업에 Rigify 자동 리그 생성은 사용하지 않았다.
- 이전에 열람한 제작자의 쇄골·twist 및 VRChat Constraint 사례는 [v025 조사](../rotation_review_v025/RESEARCH_NOTES.md)를 따른다. 이번에 영상을 추가 시청한 것은 아니다. YouTube 접근 실패를 시청/검증으로 표현하지 않는다.

본의 길이 방향과 굽힘축을 분리해야 한다는 적용 판단은 실제 Blender bone matrix와 회전 결과로 별도 검증했다. 엄지의 세 관절을 하나의 고정 월드축으로 돌리는 방식과 각 마디의 local X를 쓰는 방식은 운동 경로가 다르다. 이 비교만으로 해부학적 정답이나 VRChat 합격을 보장하지 않는다.

> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# 본 구현 참고 자료와 적용 범위

2026-09-09. 기존 상세 연구는 [v008 보고서](../v008_rigging_research/REPORT.md), [v015 참고 문헌](../v015_full_rig/REFERENCES.md)에 있다. 아래는 이번 추가 확인과 적용 판단이다. 관절 각도는 이 모델의 스트레스 테스트 값이며 의료적 정상 가동 범위를 정의한 것이 아니다.

- [OpenStax · Types of Body Movements](https://openstax.org/books/anatomy-and-physiology-2e/pages/9-5-types-of-body-movements): 본문 열람. 굽힘/폄, 벌림/모음, 회전, 전완 회내/회외, 엄지 opposition의 차이를 참고했다. 한 축으로 모든 손 기능을 재현할 수 있다고 해석하지 않았다.
- [Blender Rigify 배치 안내](https://docs.blender.org/manual/en/2.81/addons/rigging/rigify.html): 구판 공식 설명/검색 본문을 참고했다. 손가락 마디 중심·축과 골반/흉곽의 강체 구간을 확인하는 용도다. Rigify로 새 모델이나 새 대체 리그를 생성하지 않았다. 최신 안내 직접 열람은 서버 응답으로 제한되어 구판과 구분했다.
- [Blender CopyRotationConstraint API](https://docs.blender.org/api/4.0/bpy.types.CopyRotationConstraint.html): 공식 API 검색 설명 및 실제 Blender 5.2에서 공간/영향/회전 결과 검사. 본 중간 회전을 분담하지만 Unity로 제약 실행이 보존됨을 증명하지 않는다.
- [Blender Armature Modifier](https://docs.blender.org/manual/en/latest/modeling/modifiers/deform/armature.html): 최신 공식 검색 발췌만 확인. 직접 본문은 402 응답이었다. Preserve Volume은 쿼터니언 방식의 체적 보존 옵션으로 소개되지만 이번 후보의 기존 modifier를 바꾸거나 이를 엔진 검증 없이 해결책으로 채택하지 않았다.
- [VRChat Rig Requirements](https://creators.vrchat.com/avatars/rig-requirements/): 공식 본문 전체 열람. 페이지 자체에 오래된 내용이라는 경고가 있으며 SDK2 전용 경고와 SDK3 차이를 명시한다. 손가락이 없으면 무조건 현재 SDK3 IK가 불가능하다고 인용하지 않는다. 필수 몸 계층, 보조 본과 주 자식의 순서, 선택적 발가락 매핑의 점검 항목을 참고한다. 실제 최종 판정은 지원 SDK의 검사에서 해야 한다.
- [VRChat PhysBones](https://creators.vrchat.com/common-components/physbones/): 공식 본문 열람. 부모 추종과 하위 체인 물리, 한계/충돌 설정의 분리 근거다. 이번에는 체인과 수동 변형만 준비했으며 PhysBone 실행/바람 연동은 하지 않았다.
- [CMU · Optimized Centers of Rotation](https://graphics.cs.cmu.edu/?p=1436): 검색 초록 확인, 직접 접속은 시간 초과. 스키닝에서 회전 중심이 중요하다는 연구 맥락으로만 사용했다. 논문의 전체 알고리즘을 구현했다고 주장하지 않는다.

## 제작자 영상

[v008 YouTube 노트](../v008_rigging_research/YOUTUBE_NOTES.md)의 CGDive 손가락, Anthony Gibbs 어깨/팔꿈치 자료를 재사용했다. 이번 단계에 영상을 새로 끝까지 시청한 것은 아니다. 기존 설명/챕터/열람 수준은 해당 노트에 남아 있다.

- [CGDive 손가락](https://www.youtube.com/watch?v=QnCo0hzXKeQ)
- [Anthony Gibbs 어깨](https://www.youtube.com/watch?v=c_XM01I6ed0)
- [Anthony Gibbs 팔꿈치](https://www.youtube.com/watch?v=dEvWZwRxeWQ)

## 이 모델에서 확인된 적용 한계

원형을 보존한 선형 스키닝은 큰 굽힘의 접촉과 체적을 자동으로 보장하지 않는다. 실제로 면적 검사0 이후 손의 자기 교차와 코트 관통이 발견됐다. 따라서 문헌의 일반적인 좋은 배치만으로 작업 완료를 선언하지 않고, 전체 조각·다중 각도·접촉·렌더·저장 재현을 각각 검사했다. 큰 형태 변경이나 새 안구/입 메시 생성은 이번 제한에 맞지 않아 실행하지 않았다.

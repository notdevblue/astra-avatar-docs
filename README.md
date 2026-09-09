# 원형을 보존하며 실시간 아바타 만들기

기존 캐릭터 모델을 PC용 Unity / VRChat 아바타로 다듬는 과정의 연구·실험 기록입니다. 텍스처를 다시 맞추고, 본과 웨이트를 조정하고, 실제 포즈에서 결과를 비교합니다. 잘된 변화뿐 아니라 실패해서 채택하지 않은 시도와 남은 문제도 기록합니다.

**공유본 기준: 2026-09-09, v013 부모 사본 검수까지. 관절 마무리와 실제 자동 구동은 아직 미완료입니다.**

## 처음 읽는 분께

1. [현재까지의 결과와 남은 문제](STATUS.md) — 어떤 시도가 채택되었고 무엇이 미완료인지.
2. [텍스처 전후 비교](avatar_modeling/texture_v005/GALLERY.md) — 원래 형상을 유지하면서 채색을 바꾼 결과.
3. [관절 구현 연구](avatar_modeling/rigging_research_v008/REPORT.md) — 본·웨이트·면 구조와 PC 런타임의 관계.
4. [자락 8본 시험과 실패 분석](avatar_modeling/coat_rig_v011/REVIEW.md) — 독립 본을 추가해도 앉기가 자연스러워지지 않은 이유.
5. [후속 자락 개선 v012](avatar_modeling/coat_v012/REVIEW.md) — 웨이트 분배와 제어 포즈 개선, 검사 결과와 자동 구동의 차이.
6. [부모 4개 추가 사본 v013](avatar_modeling/coat_runtime_v013/REVIEW.md) — 기존 외형을 유지하며 뿌리 이동을 부모 회전으로 바꾼 비교.

## 단계별 기록

| 단계 | 주제 | 문서 |
|---|---|---|
| v001 | 가져오기, 원본 진단, 임시 리그 | [검수](avatar_modeling/inspection_v001/REVIEW.md) |
| v002 | 국소 색 보정과 몸 웨이트 | [검수](avatar_modeling/refine_v002/REVIEW.md) · [비교](avatar_modeling/refine_v002/GALLERY.md) |
| v003 | 눈 UV·국소 정점 시험 | [검수](avatar_modeling/eyes_v003/REVIEW.md) · [비교](avatar_modeling/eyes_v003/GALLERY.md) |
| v004 | 모델에 텍스처 맞추기 / 텍스처에 모델 맞추기 A/B | [검수](avatar_modeling/eyes_ab_v004/REVIEW.md) · [비교](avatar_modeling/eyes_ab_v004/GALLERY.md) · [프롬프트](avatar_modeling/eyes_ab_v004/IMAGE_PROMPT.md) |
| v005 | 레퍼런스 기준 전반 텍스처 재작업 | [검수](avatar_modeling/texture_v005/REVIEW.md) · [비교](avatar_modeling/texture_v005/GALLERY.md) · [프롬프트](avatar_modeling/texture_v005/IMAGE_PROMPTS.md) |
| v006 | 몸 웨이트와 각 무릎 한 줄 컷 | [검수](avatar_modeling/deformation_v006/REVIEW.md) · [비교](avatar_modeling/deformation_v006/GALLERY.md) |
| v007 | 사용자 피드백에 따른 관절 재검수 | [문제 분석](avatar_modeling/joint_audit_v007/REVIEW.md) |
| v008 | 관절 구현 연구와 단계별 설계 | [보고서](avatar_modeling/rigging_research_v008/REPORT.md) · [구현 계획](avatar_modeling/rigging_research_v008/IMPLEMENTATION_PLAN.md) · [검증 계획](avatar_modeling/rigging_research_v008/VALIDATION_PLAN.md) |
| v009 | 목·턱·칼라 웨이트 후보 | [전후 결과](avatar_modeling/joint_work_v009/REVIEW.md) |
| v010 | 어깨·무릎 후보 미채택, 자락 본 제안 | [검수](avatar_modeling/body_trials_v010/REVIEW.md) · [당시 제안](avatar_modeling/body_trials_v010/NEXT_SCOPE.md) |
| v011 | 자락 8본의 초기/보정 배치 시험, 두 후보 미채택 | [실험과 실패 분석](avatar_modeling/coat_rig_v011/REVIEW.md) |
| v012 | 자락 형상·웨이트 개선, 254동작 표본 검사 | [검수](avatar_modeling/coat_v012/REVIEW.md) · [추가 조사](avatar_modeling/coat_v012/RESEARCH_NOTES.md) · [자동 구동 제안](avatar_modeling/coat_v012/NEXT_AUTOMOTION_SCOPE.md) |
| v013 | 부모4개 추가, 4방법×286표본 비교와 FBX 구조 왕복검사 | [검수와 전후 비교](avatar_modeling/coat_runtime_v013/REVIEW.md) |

[YouTube 조사 노트](avatar_modeling/rigging_research_v008/YOUTUBE_NOTES.md) · [출처와 열람 범위](avatar_modeling/rigging_research_v008/SOURCES.md) · [초기 제작 워크플로 조사](docs/NANO_WORKFLOW_RESEARCH.md)

## 읽을 때 알아둘 점

각 문서는 해당 단계 당시의 판단을 담습니다. 이전 문서의 “현재”나 “권장”은 후속 결정으로 달라졌을 수 있으므로 [최신 상태](STATUS.md)를 함께 봐주세요. 수치 검증 통과와 자연스러운 외형, 사용자 채택, 실제 엔진 실행은 서로 다른 판단입니다.

이 저장소는 문서와 본문 비교 그림을 모은 공유본입니다. Blender·FBX·원본 텍스처·레퍼런스 원본·실행 코드·검증 원시 데이터는 배포하지 않습니다. 모델을 내려받아 그대로 재현하는 패키지가 아니며, 문서 속 파일명은 작업 과정을 설명하기 위한 기록입니다. [공유 범위와 갱신 방법](SHARING.md) · [파일 목록](INVENTORY.md)

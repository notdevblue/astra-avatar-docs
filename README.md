# 원형을 보존하며 실시간 아바타 만들기

기존 캐릭터 모델을 PC용 Unity / VRChat 아바타로 다듬는 과정의 연구·실험 기록입니다. 텍스처를 다시 맞추고, 본과 웨이트를 조정하고, 실제 포즈에서 결과를 비교합니다. 잘된 변화뿐 아니라 실패해서 채택하지 않은 시도와 남은 문제도 기록합니다.

**공유본 기준: 2026-09-09, v033 기존 무릎 구조 수정·검수까지. 무릎의 깊은 틈과 검사된 교차가 개선됐으며 전체 관절과 실제 자동 구동은 아직 미완료입니다.**

[최신 양 무릎 구조 수정과 검증](avatar_modeling/v033_knee_structure/REVIEW.md). 기존 표면을 보존하는6줄 컷과 넓은 변형 분담으로 전신1377/집중124/새384표본의 무릎 교차·면적 이상0,다른 부위 새 악화0입니다. 작은 각진 주름과 다른 관절은 남습니다. [이전 v032 국소 후보](avatar_modeling/v032_knee_local/REVIEW.md)는 당시 미채택 기록입니다.

[최신 토폴로지 조사와 실제 수정 비교](avatar_modeling/v031_topology/REVIEW.md) · [조사 근거와 수정 방향](avatar_modeling/v031_topology/DIRECTION.md). 두 면 컷 사본은 회귀로 미채택이며 기준은 v030입니다.

## 처음 읽는 분께

1. [현재까지의 결과와 남은 문제](STATUS.md) — 어떤 시도가 채택되었고 무엇이 미완료인지.
2. [텍스처 전후 비교](avatar_modeling/v005_texture/GALLERY.md) — 원래 형상을 유지하면서 채색을 바꾼 결과.
3. [관절 구현 연구](avatar_modeling/v008_rigging_research/REPORT.md) — 본·웨이트·면 구조와 PC 런타임의 관계.
4. [자락 8본 시험과 실패 분석](avatar_modeling/v011_coat_rig/REVIEW.md) — 독립 본을 추가해도 앉기가 자연스러워지지 않은 이유.
5. [후속 자락 개선 v012](avatar_modeling/v012_coat/REVIEW.md) — 웨이트 분배와 제어 포즈 개선, 검사 결과와 자동 구동의 차이.
6. [부모 4개 추가 사본 v013](avatar_modeling/v013_coat_runtime/REVIEW.md) — 기존 외형을 유지하며 뿌리 이동을 부모 회전으로 바꾼 비교.

[최신 무릎·오른쪽 전완 국소 검수](avatar_modeling/v030_joint_checkpoint/REVIEW.md) · [손 미채택 비교](avatar_modeling/v027_hand_axis/REVIEW.md) · [고관절 회귀](avatar_modeling/v028_rotation_followup/REVIEW.md) · [무릎 보조 중심 비교](avatar_modeling/v029_knee_pivot/REVIEW.md). 완성본이 아닙니다.

## 단계별 기록

단계 폴더는 `v001_inspection`처럼 버전 번호를 앞에 두어 이름순으로 정렬됩니다.

| 단계 | 주제 | 문서 |
|---|---|---|
| v001 | 가져오기, 원본 진단, 임시 리그 | [검수](avatar_modeling/v001_inspection/REVIEW.md) |
| v002 | 국소 색 보정과 몸 웨이트 | [검수](avatar_modeling/v002_refine/REVIEW.md) · [비교](avatar_modeling/v002_refine/GALLERY.md) |
| v003 | 눈 UV·국소 정점 시험 | [검수](avatar_modeling/v003_eyes/REVIEW.md) · [비교](avatar_modeling/v003_eyes/GALLERY.md) |
| v004 | 모델에 텍스처 맞추기 / 텍스처에 모델 맞추기 A/B | [검수](avatar_modeling/v004_eyes_ab/REVIEW.md) · [비교](avatar_modeling/v004_eyes_ab/GALLERY.md) · [프롬프트](avatar_modeling/v004_eyes_ab/IMAGE_PROMPT.md) |
| v005 | 레퍼런스 기준 전반 텍스처 재작업 | [검수](avatar_modeling/v005_texture/REVIEW.md) · [비교](avatar_modeling/v005_texture/GALLERY.md) · [프롬프트](avatar_modeling/v005_texture/IMAGE_PROMPTS.md) |
| v006 | 몸 웨이트와 각 무릎 한 줄 컷 | [검수](avatar_modeling/v006_deformation/REVIEW.md) · [비교](avatar_modeling/v006_deformation/GALLERY.md) |
| v007 | 사용자 피드백에 따른 관절 재검수 | [문제 분석](avatar_modeling/v007_joint_audit/REVIEW.md) |
| v008 | 관절 구현 연구와 단계별 설계 | [보고서](avatar_modeling/v008_rigging_research/REPORT.md) · [구현 계획](avatar_modeling/v008_rigging_research/IMPLEMENTATION_PLAN.md) · [검증 계획](avatar_modeling/v008_rigging_research/VALIDATION_PLAN.md) |
| v009 | 목·턱·칼라 웨이트 후보 | [전후 결과](avatar_modeling/v009_joint_work/REVIEW.md) |
| v010 | 어깨·무릎 후보 미채택, 자락 본 제안 | [검수](avatar_modeling/v010_body_trials/REVIEW.md) · [당시 제안](avatar_modeling/v010_body_trials/NEXT_SCOPE.md) |
| v011 | 자락 8본의 초기/보정 배치 시험, 두 후보 미채택 | [실험과 실패 분석](avatar_modeling/v011_coat_rig/REVIEW.md) |
| v012 | 자락 형상·웨이트 개선, 254동작 표본 검사 | [검수](avatar_modeling/v012_coat/REVIEW.md) · [추가 조사](avatar_modeling/v012_coat/RESEARCH_NOTES.md) · [자동 구동 제안](avatar_modeling/v012_coat/NEXT_AUTOMOTION_SCOPE.md) |
| v013 | 부모4개 추가, 4방법×286표본 비교와 FBX 구조 왕복검사 | [검수와 전후 비교](avatar_modeling/v013_coat_runtime/REVIEW.md) |
| v014 | 무릎383정점 웨이트 개선,48자세/254동작과 Unity 가져오기 검수 | [전후 결과](avatar_modeling/v014_joint_followup/REVIEW.md) · [추가 조사](avatar_modeling/v014_joint_followup/RESEARCH_NOTES.md) · [미실행 보조 본 제안](avatar_modeling/v014_joint_followup/NEXT_SCOPE.md) |
| v015 | 전신141자세 진단, 무릎/어깨4보조 본 비교 | [검수](avatar_modeling/v015_full_rig/REVIEW.md) · [참고 자료](avatar_modeling/v015_full_rig/REFERENCES.md) |
| v016–v023 | 손가락·전신 보조·헤어/넥타이/앞발 통합, 원형 보존과 실패 포함 검수 | [상세 결과](avatar_modeling/v023_rig_review/REVIEW.md) · [참고 자료](avatar_modeling/v023_rig_review/REFERENCES.md) |
| v024–v025 | 어깨 원인 분리,897복합 회전/중간 각도,370프레임 재로드 | [검수와 전후 그림](avatar_modeling/v025_rotation_review/REVIEW.md) · [추가 조사](avatar_modeling/v025_rotation_review/RESEARCH_NOTES.md) |
| v026 | 기존 정점 최대2mm와 국소 웨이트,959표본,접촉 회귀로 미채택 | [검수와 전후 그림](avatar_modeling/v026_coat_contact/REVIEW.md) |
| v027 | 손가락 축·웨이트482표본 비교, 교차 회귀로 미채택 | [검수](avatar_modeling/v027_hand_axis/REVIEW.md) · [참고](avatar_modeling/v027_hand_axis/REFERENCES.md) |
| v028 | 중간 회전 압축 감소와 코트 관통 회귀, 미채택 | [검수](avatar_modeling/v028_rotation_followup/REVIEW.md) |
| v029 | 기존 무릎 보조 중심7개×124자세 비교 | [검수](avatar_modeling/v029_knee_pivot/REVIEW.md) |
| v030 | 무릎/오른쪽 전완 국소 통합,1281몸/124무릎/494프레임 | [최신 검수](avatar_modeling/v030_joint_checkpoint/REVIEW.md) |
| v031 | 실제 변형 삼각형·면 흐름 조사와 미채택 대각선 실험 | [검수](avatar_modeling/v031_topology/REVIEW.md) |
| v032 | 양 무릎 국소 웨이트와 다음 구조 편집 제안 | [이전 비교](avatar_modeling/v032_knee_local/REVIEW.md) |
| v033 | 기존 무릎6줄 컷·넓은 분담,1377/124/새384/동작 검증 | [최신 검수](avatar_modeling/v033_knee_structure/REVIEW.md) |

[YouTube 조사 노트](avatar_modeling/v008_rigging_research/YOUTUBE_NOTES.md) · [출처와 열람 범위](avatar_modeling/v008_rigging_research/SOURCES.md) · [초기 제작 워크플로 조사](docs/NANO_WORKFLOW_RESEARCH.md)

## 읽을 때 알아둘 점

각 문서는 해당 단계 당시의 판단을 담습니다. 이전 문서의 “현재”나 “권장”은 후속 결정으로 달라졌을 수 있으므로 [최신 상태](STATUS.md)를 함께 봐주세요. 수치 검증 통과와 자연스러운 외형, 사용자 채택, 실제 엔진 실행은 서로 다른 판단입니다.

이 저장소는 문서와 본문 비교 그림을 모은 공유본입니다. Blender·FBX·원본 텍스처·레퍼런스 원본·실행 코드·검증 원시 데이터는 배포하지 않습니다. 모델을 내려받아 그대로 재현하는 패키지가 아니며, 문서 속 파일명은 작업 과정을 설명하기 위한 기록입니다. [공유 범위와 갱신 방법](SHARING.md) · [파일 목록](INVENTORY.md)

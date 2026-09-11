# 원형을 보존하며 실시간 아바타 만들기

2026-09-11 누락 기록을 보충했습니다. **이번 공유 기준은 [v186 통합 검토](avatar_modeling/v186_review_bundle/START_HERE.md)**이며, 오늘 검수가 끝난 v153–v186과 그 전에 빠졌던 v120–v152를 함께 수록했습니다. **현재 진행 중인 v187 이후는 이번 공유에서 제외했습니다.** 아래 이전 기록의 “작업 중”, “최신”은 각 단계 당시 상태이며 후속 검토 결과를 함께 읽어 주세요.

카라·헤어·소매 개선과 코트 분리, Unity 자동 보정·Humanoid 수정, 실제 Editor 성능 비교, 기존 면10곳의 후속 검증을 담았습니다. 어깨·겨드랑이·골반·손의 잔여 문제와 미채택 실험도 표시했습니다. 전체 아바타 완성이나 실제 VRChat 클라이언트 검증 완료를 뜻하지 않습니다.

2026-09-10 **[v119 옷 늘어남 후속 종합 검수](avatar_modeling/v119_garment_review/START_HERE.md)**를 추가했습니다. 옷의 방향별 늘어남까지 측정하고 기존 코트 정점의 넓은 옆들기 보정만 선별했습니다. 옆들기120 명령에서 2배 초과 넓이는9.965%→8.192%로 줄었지만, 가벼운 늘어남과 일부 눌림이 주변으로 분산되는 한계가 있습니다. 앞들기·큰 몸통 비틀림·옷 전체 형태 완료는 아닙니다. 새542·전신1377·저장동작601의 해당 검사에서 추가 경고0, 원형·무릎·웨이트·UV 유지, Unity/VRChat 자동 구동은 미검증입니다. v109–v119의 조사·실패·범위 제한·전후 그림을 한 문서에 모았으며 사용자 채택 전 검수 후보입니다. 아래는 각 이전 시점의 기록입니다.

2026-09-10 **[v108 연속 수정 종합 검수](avatar_modeling/v108_review_delivery/START_HERE.md)**를 추가했습니다. v089에 기존 소매 정점의 양쪽 옆 들기 보정2개를 더했습니다. 원래 기하·UV·웨이트·102본과 v038 SMALL 무릎을 유지하며, 새 난수500+기본30·전신1377·저장 동작601에서 새 면적/접촉 경고0을 확인했습니다. 함몰은 부분 완화됐고 큰 접힘·앞 들기·Unity 자동 구동·PhysBone은 남습니다. 앞 들기 v107 실험은 미통합입니다. v098–v108의 성공·실패와 비교 그림을 한 문서에 모았으며, 아래는 각 이전 시점의 기록입니다.

2026-09-10 **[v094–v097 조사와 13개 수정 실험](avatar_modeling/v097_local_pose_shape/START_HERE.md)**을 추가했습니다. 공식 문서·저자 글·튜토리얼을 조사하고 수정 전 계획을 기록한 뒤 단면 웨이트4안, bank 웨이트6안, 국소 포즈 키3안을 실제 검사했습니다. 모두 새 변형 경고가 있어 미채택이며 **기준은 v089 유지**입니다. 새 메시 생성·리메시 없음. 아래는 이전 시점의 기록입니다.

2026-09-10 **[v090–v093 텍스처·겨드랑이 후속 안내](avatar_modeling/v093_bake_projection/START_HERE.md)**를 추가했습니다. 재베이크 줄무늬와 검은 누락을 해결하는 색 전달 경로를 확인했습니다. 큰 주름과 새 국소 변형 경고는 남아 **기준은 v089를 유지**하며 v093은 별도 비교용입니다. 전신1377에서 무릎/수정밖 결과는 같지만 수정패치 새 계보 경고2, 저장동작601에서 패치 새6/해소19가 남았습니다. 아래는 이전 시점의 기록입니다.

2026-09-10 **최신 후보는 [v089 종합 검수 안내](avatar_modeling/v089_review_delivery/START_HERE.md)**입니다. v083에 목 여섯 정점의 최대0.3%p 웨이트 보정만 추가했습니다. 새 목1045자세의 피부 면적 경고2→1·새0, 전신1377·저장동작601에서 새 면적0입니다. 큰 어깨 홈은 남고, v084–v088의 국소 구조 실험은 제외했습니다. 특히 UV4묶음을 가로지르는 면 합치기에서 실제 텍스처 줄무늬가 생기는 원인을 기록했습니다. 아래는 각 시점의 이전 기록입니다.

2026-09-10 **최신 선별 검수는 [v083 통합 안내](avatar_modeling/v083_review_delivery/START_HERE.md)**에서 확인합니다. v070–v083의 원인 조사·별도 구조 실험·전신 검증을 정리했습니다. v068에 손15raw·뒤 목14raw의 국소 웨이트만 추가했고, 원형·기존102본·v038 SMALL 무릎을 유지했습니다. 큰 어깨 홈과 옷자락 접촉은 남습니다. 실패한 어깨/카라/노멀 실험을 검수본에 넣지 않았습니다. 아래의 이전 '최신' 표현은 각 기록 당시 상태입니다.

[옷·팔·손 작업 종합 검수 — 이 문서부터 보기](avatar_modeling/v068_upper_delivery/START_HERE.md). 확인할 파일, 전후 그림, 동작 프레임, v042–v069 전체 실험 지도, 개선·미해결 상태를 한 문서로 모았습니다. 이번 갱신은 안내 문서이며 모델 변경은 없습니다.

2026-09-10 v062–v069 결과: [v068 선별 검수본·비교·동작](avatar_modeling/v068_upper_delivery/REVIEW.md)에 손 보정·코트 연결·뒤 카라와 소매11면 대각선을 모았습니다. 새644표본에서 소매 새 면적 경고0·기존169해소, 손–소매 교차0을 확인했습니다. 저장 동작385표본 새 경고0·재로드 위치 오차0입니다. 원형과 v038 SMALL 무릎을 유지했습니다. **큰 겨드랑이 접힘·손 잔여 압축/음영은 미해결이며 전체 완료나 사용자 채택이 아닙니다.** [실제 본과 엔진 전달 요건](avatar_modeling/v068_upper_delivery/RUNTIME_HANDOFF.md), [고정 삼각형 조건의 차이](avatar_modeling/v065_frozen_triangles/REVIEW.md)도 기록했습니다. Unity/VRChat 실행은 미검증입니다.

최신 비교는 [v059 손·코트·카라 선별 검수본과 동작](avatar_modeling/v059_upper_review/REVIEW.md), [v060 엄지·손목 추가 검사](avatar_modeling/v060_hand_range/REVIEW.md), [v061 소매 웨이트의 실패 비교](avatar_modeling/v061_cuff_weights/REVIEW.md)입니다. 큰 겨드랑이 접힘과 소매 후속 작업은 진행 중입니다. 개선된 손과 유지한 원형을 실제 비교 그림으로 확인할 수 있습니다.

2026-09-10 연속 작업: [상세 조사와 모델 비교](avatar_modeling/v042_upper_research/RESEARCH.md), [손 검수 후보](avatar_modeling/v046_finger_topology/REVIEW.md), [옷 보정의 후속 검증](avatar_modeling/v056_upper_refined/REVIEW.md)을 추가했습니다. 손은 새1100표본에서 원본 면적 경고571→3으로 줄었고 전신1377에서 무릎 결과를 유지했습니다. 코트 연결·뒤 카라 정리와 여러 어깨 구조를 비교했지만 자동 옷 보정은 독립 복합 자세에서 경고가 남아 일반 회전의 최종본으로 채택하지 않았습니다. **옷·팔 작업은 계속 진행 중이며 전체 해결·Unity 구동 완료가 아닙니다. v038 SMALL 무릎 채택은 유지합니다.**

이전 검수: [v041 — 코트 옷깃 국소 웨이트와 회전 검사](avatar_modeling/v041_lapel_weights/REVIEW.md). 기존208정점/95위치의 웨이트만 작게 조정했습니다. 중립 외형·무릎을 유지했고,120도 팔 올림에서 옷깃 추적 지점 상승은 약1.47mm 줄었습니다. 집중137/전신1377/동작487표본에서 새 면적 경고0이지만, 코트 자기 접촉 집합은 달라지고 큰 어깨·겨드랑이 접힘은 남습니다. **작은 보정 후보이며 사용자 채택·전체 관절 완료는 아닙니다.**

이전 진단: [v040 — Blender MCP와 카라·목·어깨 검사](avatar_modeling/v040_mcp_inspection/REVIEW.md). 실제 MCP로 장면/뷰포트/웨이트·면을 조사했습니다. 팔 올림에서 코트 옷깃이 쇄골·상완에 끌려 뾰족하게 꺾이고, 목 회전의 각진 경계·뒤 카라 요철도 남습니다.24자세 면적 경고0을 형상 합격으로 해석하지 않습니다. **모델을 보정한 단계가 아닌 검사 사본과 계획이며, v039 후보·v038 SMALL 무릎 채택 상태는 유지합니다.**

이전 실행: [v039 — 팔꿈치·전완 국소 보정과 어깨 검수](avatar_modeling/v039_arm_local/REVIEW.md). 136 raw 정점/63위치 웨이트와 왼팔꿈치 기존 두 면의 대각선만 수정했습니다. 정점 이동·새 메시·새 본은 없으며 채택한 v038 무릎을 유지합니다. 전신·복합·격자·저장 동작에서 새 면적 경고0입니다. **검수 후보이며 어깨의 큰 주름·소매 자기 접촉과 전체 관절·Unity 작업은 남습니다.** 왼어깨 웨이트 시험은 원래대로 되돌렸습니다.

무릎 채택 기준: [v038 — 뒤쪽 접힘을 유지한 앞면 미세 보정](avatar_modeling/v038_knee_front/REVIEW.md). v030에서 앞쪽38위치의 웨이트만 작게 조정했고120도 최대 이동은2.574mm입니다. 뒤쪽 위치와 원래 교차 면 쌍을 유지하며,1377/124/384/재생365표본에서 새 교차·면적 경고가 추가되지 않았습니다. **사용자가 v038 SMALL을 무릎 보정본으로 채택했습니다. 이후 관련 작업의 기준으로 사용하며 v030 비교 기준·v037 미채택을 유지합니다.** 원래 안쪽 주름·교차는 남습니다.

기존 캐릭터 모델을 PC용 Unity / VRChat 아바타로 다듬는 과정의 연구·실험 기록입니다. 텍스처를 다시 맞추고, 본과 웨이트를 조정하고, 실제 포즈에서 결과를 비교합니다. 잘된 변화뿐 아니라 실패해서 채택하지 않은 시도와 남은 문제도 기록합니다.

**공유본 기준: 2026-09-10, v042–v069 연구·실행·미채택 실험까지, v038 SMALL 무릎 채택 유지. 전체 관절·VRChat 구동은 미완료입니다. 아래는 각 이전 단계 당시 기록입니다.**

[v036 안쪽 홈 수정·비교](avatar_modeling/v036_knee_crease/REVIEW.md).35개 실험에서 본 보정은 수치 검사를 통과해도 홈이 넓어져 미채택했습니다. 기존 정점 자세 보정과 기존 면 컷은 측정 단면의 빈 구간을17.11→8.12mm로 줄였습니다. 원래98본/기존 좌표·웨이트 유지,791점·871면 컷 추가와6자세 키입니다. 전신1377/새256/재생365표본의 무릎 교차0이지만 면적 경고가 남고 Blender 드라이버만 구현되어 최종 채택하지 않았습니다. 아래는 이전 단계 당시 기록입니다.

[v034 관절 두께 연구·진단](avatar_modeling/v034_joint_volume_research/REPORT.md) · [개선 실행 계획](avatar_modeling/v034_joint_volume_research/IMPLEMENTATION_PLAN.md). 당시에는 모델을 수정하지 않고 임시 DQ 비교로 LBS 수축의 기여를 확인했습니다. 중심 두께와 접힘을 분리하는 방향을 정리했고 아래 v035에서 실행했습니다. [v033 검수](avatar_modeling/v033_knee_structure/REVIEW.md)의 교차 검사 통과는 자연스러운 형상 합격과 다릅니다.

[v035 계획 실행 결과와 비교 그림](avatar_modeling/v035_knee_volume/REVIEW.md).12개 수정 후보를 비교해 짧은 보조 관절과 뒤쪽 분담 후보를 남겼습니다.1377/124/384/새256자세와 재생365표본에서 무릎 교차·면적 이상0입니다. 굽힘 두께는 개선됐지만 작은 각짐과 다른 관절은 남습니다. v030/v033은 비교용으로 보존하며 사용자 최종 채택과 구분합니다.

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
| v034 | 관절 두께 연구·LBS/DQ 읽기 전용 비교, 개선안 기록 | [연구와 진단](avatar_modeling/v034_joint_volume_research/REPORT.md) · [개선 계획](avatar_modeling/v034_joint_volume_research/IMPLEMENTATION_PLAN.md) |
| v035 | 기존 메시 유지·보조4본, 무릎 두께/접촉 후보와 회귀 검증 | [실행 결과](avatar_modeling/v035_knee_volume/REVIEW.md) |
| v036 | 안쪽 홈35사본 비교, 기존 정점 자세 보정 시제품과 잔여 압축 | [실행 결과](avatar_modeling/v036_knee_crease/REVIEW.md) |
| v037 | v030 국소 목표·기존 웨이트와5면 대각선 수정, 좁은 접힘·두께 비교 | [실행 결과](avatar_modeling/v037_knee_target/REVIEW.md) |
| v038 | v030 뒤쪽 고정, 앞면38위치의 작은 웨이트 보정 비교 | [실행 결과](avatar_modeling/v038_knee_front/REVIEW.md) |
| v039 | 팔꿈치·전완 국소 웨이트, 기존 두 면 대각선 고정과 어깨/회전 검수 | [실행 결과](avatar_modeling/v039_arm_local/REVIEW.md) |
| v040 | Blender MCP 연결·카라/목/어깨 상태 진단과 후속 계획 | [진단 결과](avatar_modeling/v040_mcp_inspection/REVIEW.md) |
| v041 | 코트 옷깃 국소 웨이트와 회전 검사 | [검수](avatar_modeling/v041_lapel_weights/REVIEW.md) |
| v042 | 옷·팔·손 연구와 실제 모델 비교 | [기록](avatar_modeling/v042_upper_research/RESEARCH.md) |
| v043 | 엄지 축과 손가락 웨이트 | [기록](avatar_modeling/v043_upper_work/REVIEW.md) |
| v044 | 기존 코트 자세 목표 | [기록](avatar_modeling/v044_pose_shapes/REVIEW.md) |
| v045 | 코트 정점 연결과 입력 정정 | [기록](avatar_modeling/v045_coat_connections/REVIEW.md) |
| v046 | 손 관절·웨이트 최종 후보 | [기록](avatar_modeling/v046_finger_topology/REVIEW.md) |
| v047 | 넓은 어깨 지지 본 시험 | [기록](avatar_modeling/v047_coat_support/REVIEW.md) |
| v048 | Corrective Smooth와 DQS 비교 | [기록](avatar_modeling/v048_clothing_correctives/REVIEW.md) |
| v049 | 어깨 2/4줄 컷 시험 | [기록](avatar_modeling/v049_arm_loops/REVIEW.md) |
| v050 | 어깨 웨이트 최적화 | [기록](avatar_modeling/v050_shoulder_fit/REVIEW.md) |
| v051 | 노멀·연결 분리 진단 | [기록](avatar_modeling/v051_pose_normals/REVIEW.md) |
| v052 | 제한한 자동 옷 자세 보정 | [기록](avatar_modeling/v052_pose_clothing/REVIEW.md) |
| v053 | 손·코트 자동 노멀 비교 | [기록](avatar_modeling/v053_surface_finish/REVIEW.md) |
| v054 | 뒤 카라 0.5mm 정리 | [기록](avatar_modeling/v054_collar_local/REVIEW.md) |
| v055 | 통합 동작과 중간값 검사 | [기록](avatar_modeling/v055_upper_delivery/REVIEW.md) |
| v056 | 새 표본으로 확인한 자세키 한계 | [기록](avatar_modeling/v056_upper_refined/REVIEW.md) |
| v057 | 여러 자세의 기본 정점 조정 | [기록](avatar_modeling/v057_coat_rest_fit/REVIEW.md) |
| v058 | 좁은 어깨 보조본과 미채택 판정 | [기록](avatar_modeling/v058_shoulder_volume/REVIEW.md) |
| v059 | 손·코트·카라 선별 검수와 동작 | [기록](avatar_modeling/v059_upper_review/REVIEW.md) |
| v060 | 엄지 맞섬·손목 복합 범위 | [기록](avatar_modeling/v060_hand_range/REVIEW.md) |
| v061 | 소매 웨이트와 새 교차 비교 | [기록](avatar_modeling/v061_cuff_weights/REVIEW.md) |

[YouTube 조사 노트](avatar_modeling/v008_rigging_research/YOUTUBE_NOTES.md) · [출처와 열람 범위](avatar_modeling/v008_rigging_research/SOURCES.md) · [초기 제작 워크플로 조사](docs/NANO_WORKFLOW_RESEARCH.md)

## 읽을 때 알아둘 점

각 문서는 해당 단계 당시의 판단을 담습니다. 이전 문서의 “현재”나 “권장”은 후속 결정으로 달라졌을 수 있으므로 [최신 상태](STATUS.md)를 함께 봐주세요. 수치 검증 통과와 자연스러운 외형, 사용자 채택, 실제 엔진 실행은 서로 다른 판단입니다.

이 저장소는 문서와 본문 비교 그림을 모은 공유본입니다. Blender·FBX·원본 텍스처·레퍼런스 원본·실행 코드·검증 원시 데이터는 배포하지 않습니다. 모델을 내려받아 그대로 재현하는 패키지가 아니며, 문서 속 파일명은 작업 과정을 설명하기 위한 기록입니다. [공유 범위와 갱신 방법](SHARING.md) · [파일 목록](INVENTORY.md)

## v062–v069 후속 기록

| 단계 | 문서 |
|---|---|
| v062 | [검수](avatar_modeling/v062_sleeve_diagonals/REVIEW.md) |
| v063 | [검수](avatar_modeling/v063_sleeve_fit/REVIEW.md) |
| v064 | [검수](avatar_modeling/v064_shoulder_concavity/REVIEW.md) |
| v065 | [검수](avatar_modeling/v065_frozen_triangles/REVIEW.md) |
| v066 | [검수](avatar_modeling/v066_shoulder_profile/REVIEW.md) |
| v067 | [검수](avatar_modeling/v067_hand_normals/REVIEW.md) |
| v068 | [검수](avatar_modeling/v068_upper_delivery/REVIEW.md) |
| v069 | [검수](avatar_modeling/v069_hand_mcp_volume/REVIEW.md) |

## v070–v083 후속 기록

| 단계 | 문서 |
|---|---|
| v070 | [기록](avatar_modeling/v070_upper_cause/REVIEW.md) |
| v071 | [기록](avatar_modeling/v071_surface_strain/REVIEW.md) |
| v072 | [기록](avatar_modeling/v072_compact_front/REVIEW.md) |
| v073 | [기록](avatar_modeling/v073_hand_residual/REVIEW.md) |
| v074 | [기록](avatar_modeling/v074_shoulder_graph/REVIEW.md) |
| v075 | [기록](avatar_modeling/v075_coat_clearance/REVIEW.md) |
| v076 | [기록](avatar_modeling/v076_hand_surface/REVIEW.md) |
| v077 | [기록](avatar_modeling/v077_shoulder_diagonals/REVIEW.md) |
| v078 | [기록](avatar_modeling/v078_sleeve_rest/REVIEW.md) |
| v079 | [기록](avatar_modeling/v079_collar_tail_review/REVIEW.md) |
| v080 | [기록](avatar_modeling/v080_neck_weights/REVIEW.md) |
| v081 | [기록](avatar_modeling/v081_continued_review/REVIEW.md) |
| v082 | [기록](avatar_modeling/v082_shoulder_pivot/REVIEW.md) |
| v083 | [기록](avatar_modeling/v083_review_delivery/REVIEW.md) |

## v084–v089 국소 구조·목 후속 기록

| 단계 | 문서 |
|---|---|
| v084 | [기록](avatar_modeling/v084_armhole_flow/REVIEW.md) |
| v085 | [기록](avatar_modeling/v085_fold_landmarks/REVIEW.md) |
| v086 | [기록](avatar_modeling/v086_fold_cleanup/REVIEW.md) |
| v087 | [기록](avatar_modeling/v087_neck_residual/REVIEW.md) |
| v088 | [기록](avatar_modeling/v088_fold_tessellation/REVIEW.md) |
| v089 | [기록](avatar_modeling/v089_review_delivery/REVIEW.md) |

## v090–v093 텍스처와 국소 면 연결

| 단계 | 문서 |
|---|---|
| v090 | [기록](avatar_modeling/v090_patch_rebake/REVIEW.md) |
| v091 | [기록](avatar_modeling/v091_patch_flow/REVIEW.md) |
| v092 | [기록](avatar_modeling/v092_patch_guard/REVIEW.md) |
| v093 | [기록](avatar_modeling/v093_bake_projection/REVIEW.md) |

## v094–v097 어깨 재조사

| 단계 | 문서 |
|---|---|
| v094 | [기록](avatar_modeling/v094_shoulder_research/RESEARCH.md) |
| v095 | [기록](avatar_modeling/v095_section_weights/REVIEW.md) |
| v096 | [기록](avatar_modeling/v096_bank_weights/REVIEW.md) |
| v097 | [기록](avatar_modeling/v097_local_pose_shape/REVIEW.md) |


## v098–v108 소매 표면과 접촉 조건

| 단계 | 문서 |
|---|---|
| v098 | [목표 표면](avatar_modeling/v098_surface_target/REVIEW.md) |
| v099 | [다중 자세 면 제약](avatar_modeling/v099_surface_guard/REVIEW.md) |
| v100 | [접촉과 대각선 전환](avatar_modeling/v100_surface_contact/REVIEW.md) |
| v101 | [오른쪽 독립 보정](avatar_modeling/v101_right_surface/REVIEW.md) |
| v102 | [영역 내부 간격](avatar_modeling/v102_bilateral_guard/REVIEW.md) |
| v103 | [통합·경계 보호](avatar_modeling/v103_bilateral_review/REVIEW.md) |
| v104 | [영역 밖 고정 면](avatar_modeling/v104_right_contact/REVIEW.md) |
| v105 | [새 회전의 늘어남 진단](avatar_modeling/v105_surface_review/REVIEW.md) |
| v106 | [검증된 소매 보정](avatar_modeling/v106_strain_margin/REVIEW.md) |
| v107 | [앞 들기·음영 분리 시험](avatar_modeling/v107_front_surface/REVIEW.md) |
| v108 | [전체 결과·파일·26마커·남은 문제](avatar_modeling/v108_review_delivery/START_HERE.md) |
| v109 | [옷 전체 절대 늘어남·원문 조사](avatar_modeling/v109_garment_strain/REVIEW.md) |
| v110 | [넓은 기존점 변형률 목표6종](avatar_modeling/v110_cloth_metric/REVIEW.md) |
| v111 | [목표 자세의 충돌 구간 보호](avatar_modeling/v111_cloth_guard/REVIEW.md) |
| v112 | [앞·옆 포즈 키 혼합 회전 실패](avatar_modeling/v112_garment_pose/REVIEW.md) |
| v113 | [연속 범위 보호5회·선별검증](avatar_modeling/v113_garment_range/REVIEW.md) |
| v114 | [몸판·자락 전체 들림의 형태 실패](avatar_modeling/v114_garment_lift/REVIEW.md) |
| v115 | [새 표본에서 앞들기 제외](avatar_modeling/v115_garment_review/START_HERE.md) |
| v116 | [옆들기 잔여 면 진단](avatar_modeling/v116_garment_review/START_HERE.md) |
| v117 | [몸통 복합 회전의 한계](avatar_modeling/v117_garment_review/START_HERE.md) |
| v118 | [몸통 조건·드라이버 검증](avatar_modeling/v118_garment_review/START_HERE.md) |
| v119 | [모델·비교·동작·남은 옷 문제 종합](avatar_modeling/v119_garment_review/START_HERE.md) |

## v120–v186 누락 보충 기록

| 단계 | 문서 |
|---|---|
| v120 | [앞뒤 회전 경로 진단](avatar_modeling/v120_front_axis/REVIEW.md) |
| v121 | [팔 앞뒤 회전축 검수](avatar_modeling/v121_arm_controls/START_HERE.md) |
| v122 | [앞들기 수축 보간 진단](avatar_modeling/v122_forward_surface/REVIEW.md) |
| v123 | [기존 코트의 자기 충돌 포함 물리 목표 실험](avatar_modeling/v123_cloth_targets/REVIEW.md) |
| v124 | [원단 계산에서 얻은 앞들기 보정 목표 선별](avatar_modeling/v124_front_guard/REVIEW.md) |
| v125 | [앞들기 자동 보정 시험, 통합 보류](avatar_modeling/v125_front_pose/REVIEW.md) |
| v126 | [앞으로 드는 팔의 관절 중심·쇄골·웨이트 점검](avatar_modeling/v126_shoulder_audit/START_HERE.md) |
| v127 | [관절 깊이와 코트 웨이트 결합 시험](avatar_modeling/v127_shoulder_trials/REVIEW.md) |
| v128 | [기존 옆·뒤 표면을 목표로 한 웨이트 적합](avatar_modeling/v128_shoulder_fit/REVIEW.md) |
| v129 | [앞들기에서만 적용하는 코트 경계 보정](avatar_modeling/v129_front_boundary/REVIEW.md) |
| v130 | [앞들기 경계 보정의 회귀 면 보호](avatar_modeling/v130_boundary_guard/REVIEW.md) |
| v131 | [저장 모션에서 추가 회귀 발견](avatar_modeling/v131_shoulder_review/REVIEW.md) |
| v132 | [어깨 전진 동작 수정과 코트 경계 보정 검수](avatar_modeling/v132_shoulder_review/START_HERE.md) |
| v133 | [표면 세부 운반 목표 6종](avatar_modeling/v133_surface_transport/REVIEW.md) |
| v134 | [앞들기 표면 보정의 적용 범위 제한](avatar_modeling/v134_transport_keys/REVIEW.md) |
| v135 | [소매 찌그러짐 추가 보정 검수](avatar_modeling/v135_transport_review/START_HERE.md) |
| v136 | [기본/앞/뒤 어깨 폭 진단](avatar_modeling/v136_shoulder_width/REVIEW.md) |
| v137 | [앞90/뒤35 폭 보정과 적용 제한](avatar_modeling/v137_width_correction/REVIEW.md) |
| v138 | [앞뒤 팔 동작의 어깨 폭 비교·수정](avatar_modeling/v138_width_review/START_HERE.md) |
| v139 | [팔 앞뒤 동작의 끊긴 선·텍스처·음영 원인 진단](avatar_modeling/v139_seam_diagnosis/START_HERE.md) |
| v140 | [어깨의 각진 음영 정리](avatar_modeling/v140_shading_fix/REVIEW.md) |
| v141 | [어깨에 그려진 검은 띠 국소 보정](avatar_modeling/v141_texture_fix/REVIEW.md) |
| v142 | [겨드랑이 압축 면 국소 복원 실험](avatar_modeling/v142_fold_trials/REVIEW.md) |
| v143 | [텍스처 UV 여백 보완 비교안](avatar_modeling/v143_seam_review/START_HERE.md) |
| v144 | [정장 어깨선을 유지하면서 변형 원인 확인](avatar_modeling/v144_seam_transport/START_HERE.md) |
| v145 | [중간 실험 — 후속 검수는 v146](avatar_modeling/v145_seam_review/START_HERE.md) |
| v146 | [정장 어깨선 보존·변형·대각선 검수](avatar_modeling/v146_seam_topology/START_HERE.md) |
| v147 | [팔 전방 거상에서 어깨가 납작해지는 원인](avatar_modeling/v147_shoulder_volume/START_HERE.md) |
| v148 | [소매산 두께와 회전 억제 비교 실험](avatar_modeling/v148_volume_trials/START_HERE.md) |
| v149 | [연속 두께 복원과 접촉 제한 실험](avatar_modeling/v149_volume_fit/START_HERE.md) |
| v150 | [면 고정 방식의 검증과 미채택](avatar_modeling/v150_volume_review/START_HERE.md) |
| v151 | [부피 복원 후보의 중간 검증](avatar_modeling/v151_volume_review/START_HERE.md) |
| v152 | [팔을 앞으로 뻗을 때 어깨 두께 복원](avatar_modeling/v152_volume_review/START_HERE.md) |
| v153 | [Lapwing 구매 아바타 구조 조사](avatar_modeling/v153_lapwing_reference/START_HERE.md) |
| v154 | [어깨 지지 본·단계 회전 비교, 미채택](avatar_modeling/v154_shoulder_support/START_HERE.md) |
| v155 | [다자세 웨이트 맞춤, 미채택](avatar_modeling/v155_weight_fit/START_HERE.md) |
| v156 | [비앙카·Lapwing·SiuSiu 구조 비교](avatar_modeling/v156_three_avatar_comparison/START_HERE.md) |
| v157 | [Lapwing·SiuSiu 관절을 실제로 움직여 비교](avatar_modeling/v157_reference_pose/START_HERE.md) |
| v158 | [카라·헤어·의상 물리 조사와 적용 계획](avatar_modeling/v158_dynamics_research/RESEARCH.md) |
| v159 | [카라·라펠이 팔에 끌리는 현상 완화](avatar_modeling/v159_collar_support/START_HERE.md) |
| v160 | [원래 헤어 형태를 보존하는 PhysBone 준비와 실제 Unity 검사](avatar_modeling/v160_hair_dynamics/START_HERE.md) |
| v161 | [카라·헤어 통합 및 코트 객체 분리](avatar_modeling/v161_separate_coat/START_HERE.md) |
| v162 | [전완 Twist 분산 실험, 통합 보류](avatar_modeling/v162_forearm_twist/START_HERE.md) |
| v163 | [비앙카 v163 — 이번 작업 한 번에 확인하기](avatar_modeling/v163_review_bundle/START_HERE.md) |
| v164 | [비앙카 관절·의상·Unity 통합 조사와 실행 계획](avatar_modeling/v164_integration_research/RESEARCH_AND_PLAN.md) |
| v165 | [손목과 정장 소매 끝의 회전 역할 분리](avatar_modeling/v165_cuff_roles/START_HERE.md) |
| v166 | [어깨 국소 변형 실험, 아직 미채택](avatar_modeling/v166_shoulder_local/START_HERE.md) |
| v167 | [Unity PC 통합·실제 SDK 초기 검증 이력](avatar_modeling/v167_unity_integration/START_HERE.md) |
| v168 | [고정 삼각면 비교, 아직 미채택](avatar_modeling/v168_fixed_coat/START_HERE.md) |
| v169 | [원래 오목한 주름을 채우는 국소 실험, 미채택](avatar_modeling/v169_rest_fold/START_HERE.md) |
| v170 | [자동 보정·바람·Humanoid 통합 검사와 후속 결과](avatar_modeling/v170_runtime_validation/START_HERE.md) |
| v171 | [기존 어깨 면 연결 회전, 별도 실험](avatar_modeling/v171_edge_flow/START_HERE.md) |
| v172 | [기존 두 면 연결만 수정한 후보](avatar_modeling/v172_local_topology/START_HERE.md) |
| v173 | [자동 보정 계산 단계 축소](avatar_modeling/v173_fast_correctives/START_HERE.md) |
| v174 | [연속 통합 시험과 새로 발견한 결함](avatar_modeling/v174_continuous_qa/START_HERE.md) |
| v175 | [실제 Humanoid 기준 자세·손가락 매핑 수정](avatar_modeling/v175_humanoid_reference/START_HERE.md) |
| v176 | [검증된 코트 연결을 Unity에 반영](avatar_modeling/v176_unity_surface/START_HERE.md) |
| v177 | [lilToon Soft 검토 재질 채택](avatar_modeling/v177_material_review/START_HERE.md) |
| v178 | [동작 중 오목한 홈 완화, 세 후보 미채택](avatar_modeling/v178_pose_fold/START_HERE.md) |
| v179 | [기존 지지선 추가 시험, 미채택](avatar_modeling/v179_joint_support_cuts/START_HERE.md) |
| v180 | [비앙카 통합 검토 v180](avatar_modeling/v180_integrated_review/START_HERE.md) |
| v181 | [미해결 경고의 부위 분류](avatar_modeling/v181_joint_localization/START_HERE.md) |
| v182 | [골반 지지 후보 — 미채택](avatar_modeling/v182_pelvis_weights/START_HERE.md) |
| v183 | [어깨 국소 웨이트 최적화 — 미채택](avatar_modeling/v183_shoulder_local_weights/START_HERE.md) |
| v184 | [기존 면의 삼각 분할 수정 — 최종10곳 반영](avatar_modeling/v184_body_diagonals/START_HERE.md) |
| v185 | [인접 손 삼각면 연결 후보 — 미채택](avatar_modeling/v185_web_edgeflow/START_HERE.md) |
| v186 | [이번 개선을 한 번에 확인](avatar_modeling/v186_review_bundle/START_HERE.md) |

# 전체 단계 색인

**[v348 상세 작업 보고 — 부위별 전후·시도·미채택 이유·남은 일](avatar_modeling/v348_final_npr_review/v348_DETAILED_WORK_REPORT.md)**: 17개 절에서 계획과 실제 수행을 대조하고, UV 유지 이유·전사 범위·셰이더 비교·헤어/넥타이 수정·검증 한계를 사진과 GIF로 설명합니다. 이번 갱신은 문서 보강이며 모델은 기존 v348 검수본 그대로입니다.

2026-09-12 · [v322 텍스처 재작업·UV 경계 정리](avatar_modeling/v322_texture_rework/v322_TEXTURE_REVIEW.md): 코트·바지·셔츠 재채색, 투영 색 혼입과 패딩 덮어쓰기 수정. 원형·UV 좌표 보존, Unity 실제 전후 사진과 미채택 시험을 기록했습니다. 뒤목의 희미한 윤곽과 셔츠 세부는 남아 최종 채색 채택은 별도입니다.

2026-09-12 · [v321 주름 출처와 측면 휘어짐](avatar_modeling/v321_arm_curve_audit/README.md): 같은 자세에서 원형 전완 주름과 최근 윗소매의 추가 꺾임을 구분했습니다. 본은 일직선이나 소매 표면은 실제로 달라졌습니다. 모델 수정 없이 사진③을 보존한 비교입니다.

2026-09-12 · [v320 팔을 완전히 편 정면·측면 비교](avatar_modeling/v320_straight_arm/README.md): 사진③의 어깨 방향을 유지하고 팔꿈치를 약9.23°→0°로 편 사진을 직접 확인했습니다. 임시 기존 메시 사본의 자세 비교이며 전체 자동 구동 완료는 아닙니다.

2026-09-12 · [v319 문제·시도·미채택 사유](avatar_modeling/v319_issue_review/v319_ISSUES_AND_ATTEMPTS.md): Mac 작업의 주요 시도와 제외·보류 이유, 직접 확인한 사진6장, 부피·접촉 수치의 한계와 피드백 항목을 정리했습니다. 문서 정리 단계이며 새 모델 수정은 없습니다. 형상 문제 미해결·후보 미채택 상태로 사용자 피드백을 기다립니다.

작업 흐름과 주요 판단은 [README](README.md), 현재 공유 기준은 [v186 종합 검토](avatar_modeling/v186_review_bundle/START_HERE.md)에서 먼저 읽을 수 있습니다. 같은 문제가 여러 버전에서 어떻게 이어졌는지는 [문제별 작업 추적](PROGRESS.md)의 T01–T14를 사용하세요. 이 페이지는 자세한 보고서를 찾아볼 때 사용합니다.

2026-09-11 공유 범위인 v186까지를 버전 순서로 정리했습니다. v187 이후 진행 중 작업은 포함하지 않습니다. 표의 버전은 채택 순위가 아니며, 미채택 실험과 중간 검증도 포함합니다. 따로 공유하지 않은 단계는 종합 보고서 안에서 다룹니다.

`START_HERE`는 종합 안내, `REVIEW`는 검토 결과, `RESEARCH`는 조사, `PLAN`은 당시 실행 계획입니다. 계획에 적힌 작업이 모두 완료됐다는 뜻은 아니므로 같은 단계의 결과 및 [상태 이력](STATUS.md)을 함께 확인해 주세요.

## v001–v007 · 원본 진단과 텍스처

| 단계 | 먼저 읽을 문서 | 추가 자료 |
|---|---|---|
| v001 | [새 Bianca 모델 — 가져오기·외형·변형 검수](avatar_modeling/v001_inspection/REVIEW.md) | — |
| v002 | [Bianca — 국소 색 보정·몸 웨이트 수정 v002](avatar_modeling/v002_refine/REVIEW.md) | [비교 갤러리](avatar_modeling/v002_refine/GALLERY.md) |
| v003 | [눈 국소 정렬 시험 v003 — 미채택 비교본](avatar_modeling/v003_eyes/REVIEW.md) | [비교 갤러리](avatar_modeling/v003_eyes/GALLERY.md) |
| v004 | [눈 정렬 A/B 비교 — 새 텍스처와 기존 눈 정점 수정](avatar_modeling/v004_eyes_ab/REVIEW.md) | [비교 갤러리](avatar_modeling/v004_eyes_ab/GALLERY.md) · [IMAGE PROMPT](avatar_modeling/v004_eyes_ab/IMAGE_PROMPT.md) |
| v005 | [레퍼런스 기준 텍스처 재작업 v005](avatar_modeling/v005_texture/REVIEW.md) | [비교 갤러리](avatar_modeling/v005_texture/GALLERY.md) · [IMAGE PROMPTS](avatar_modeling/v005_texture/IMAGE_PROMPTS.md) |
| v006 | [새 텍스처 + 국소 몸 웨이트·무릎 컷 검수본 v006](avatar_modeling/v006_deformation/REVIEW.md) | [비교 갤러리](avatar_modeling/v006_deformation/GALLERY.md) |
| v007 | [v006 관절 재검수 — 사용자 피드백 반영](avatar_modeling/v007_joint_audit/REVIEW.md) | — |

## v008–v030 · 전신 본과 관절·자락

| 단계 | 먼저 읽을 문서 | 추가 자료 |
|---|---|---|
| v008 | [Bianca 관절 구현 조사와 수정 설계](avatar_modeling/v008_rigging_research/REPORT.md) | [IMPLEMENTATION PLAN](avatar_modeling/v008_rigging_research/IMPLEMENTATION_PLAN.md) · [SOURCES](avatar_modeling/v008_rigging_research/SOURCES.md) · [VALIDATION PLAN](avatar_modeling/v008_rigging_research/VALIDATION_PLAN.md) · [YOUTUBE NOTES](avatar_modeling/v008_rigging_research/YOUTUBE_NOTES.md) |
| v009 | [v009 관절 단계 작업 — 목·턱·칼라](avatar_modeling/v009_joint_work/REVIEW.md) | — |
| v010 | [v010 어깨·무릎 시험과 자락 리그 제안](avatar_modeling/v010_body_trials/REVIEW.md) | [NEXT SCOPE](avatar_modeling/v010_body_trials/NEXT_SCOPE.md) |
| v011 | [재킷 자락 8본 사본 시험 v011 — 구현·검증 완료, 두 후보 미채택](avatar_modeling/v011_coat_rig/REVIEW.md) | — |
| v012 | [자락 v012 — 검사한 동작에서 개선된 형상 후보](avatar_modeling/v012_coat/REVIEW.md) | [NEXT AUTOMOTION SCOPE](avatar_modeling/v012_coat/NEXT_AUTOMOTION_SCOPE.md) · [추가 조사](avatar_modeling/v012_coat/RESEARCH_NOTES.md) |
| v013 | [부모 4개 추가 사본과 기존 자락 비교](avatar_modeling/v013_coat_runtime/REVIEW.md) | — |
| v014 | [무릎 국소 웨이트 개선과 Unity 가져오기 검수](avatar_modeling/v014_joint_followup/REVIEW.md) | [NEXT SCOPE](avatar_modeling/v014_joint_followup/NEXT_SCOPE.md) · [추가 조사](avatar_modeling/v014_joint_followup/RESEARCH_NOTES.md) |
| v015 | [v015 전신 진단과4보조 본 사본 비교](avatar_modeling/v015_full_rig/REVIEW.md) | [참고 자료](avatar_modeling/v015_full_rig/REFERENCES.md) |
| v023 | [기존 메시 유지 · 전신 본 검수 사본 v023](avatar_modeling/v023_rig_review/REVIEW.md) | [참고 자료](avatar_modeling/v023_rig_review/REFERENCES.md) |
| v025 | [어깨 함몰 보정과 복합 회전 검수 v025](avatar_modeling/v025_rotation_review/REVIEW.md) | [추가 조사](avatar_modeling/v025_rotation_review/RESEARCH_NOTES.md) |
| v026 | [코트 앞 허리의 국소 관통 시험 v026](avatar_modeling/v026_coat_contact/REVIEW.md) | — |
| v027 | [손가락 회전축·웨이트 비교 v027](avatar_modeling/v027_hand_axis/REVIEW.md) | [참고 자료](avatar_modeling/v027_hand_axis/REFERENCES.md) |
| v028 | [중간 회전 웨이트 보정과 회귀 v028](avatar_modeling/v028_rotation_followup/REVIEW.md) | — |
| v029 | [기존 무릎 보조 본 회전 중심 비교 v029](avatar_modeling/v029_knee_pivot/REVIEW.md) | — |
| v030 | [관절 후속 검수와 제한적 통합 v030](avatar_modeling/v030_joint_checkpoint/REVIEW.md) | — |

## v031–v041 · 무릎 기준과 상체 재점검

| 단계 | 먼저 읽을 문서 | 추가 자료 |
|---|---|---|
| v031 | [토폴로지 재검사와 국소 대각선 시험 v031](avatar_modeling/v031_topology/REVIEW.md) | [DIRECTION](avatar_modeling/v031_topology/DIRECTION.md) |
| v032 | [무릎 국소 수정과 실제 관절 검수 v032](avatar_modeling/v032_knee_local/REVIEW.md) | — |
| v033 | [기존 무릎 구조 수정과 변형 분담 재설계](avatar_modeling/v033_knee_structure/REVIEW.md) | — |
| v034 | [관절 두께 손실 조사와 현재 리그 비교](avatar_modeling/v034_joint_volume_research/REPORT.md) | [IMPLEMENTATION PLAN](avatar_modeling/v034_joint_volume_research/IMPLEMENTATION_PLAN.md) |
| v035 | [짧은 보조 관절로 무릎 두께 유지](avatar_modeling/v035_knee_volume/REVIEW.md) | — |
| v036 | [무릎 안쪽 홈: 본 보정과 자세 보정 비교](avatar_modeling/v036_knee_crease/REVIEW.md) | [RESTART FROM V030](avatar_modeling/v036_knee_crease/RESTART_FROM_V030.md) |
| v037 | [v030에서 무릎만 다시 조절한 검수 사본](avatar_modeling/v037_knee_target/REVIEW.md) | — |
| v038 | [v030의 뒤쪽 접힘을 유지한 앞면 미세 보정](avatar_modeling/v038_knee_front/REVIEW.md) | — |
| v039 | [팔꿈치·전완 국소 보정과 어깨 회전 검수](avatar_modeling/v039_arm_local/REVIEW.md) | — |
| v040 | [Blender MCP 연결 시험과 카라·목·어깨 상태 진단](avatar_modeling/v040_mcp_inspection/REVIEW.md) | — |
| v041 | [코트 옷깃 국소 웨이트 보정과 회전 검증](avatar_modeling/v041_lapel_weights/REVIEW.md) | — |

## v042–v093 · 손·소매·어깨의 국소 수정

| 단계 | 먼저 읽을 문서 | 추가 자료 |
|---|---|---|
| v042 | [옷·팔·손의 변형 구조와 개선 설계](avatar_modeling/v042_upper_research/RESEARCH.md) | — |
| v043 | [엄지 축과 손가락 웨이트의 원인 분리](avatar_modeling/v043_upper_work/REVIEW.md) | [ACTUAL CHECK NOTES](avatar_modeling/v043_upper_work/ACTUAL_CHECK_NOTES.md) · [PROGRESS](avatar_modeling/v043_upper_work/PROGRESS.md) |
| v044 | [원래 옷의 자세 목표를 만드는 시험](avatar_modeling/v044_pose_shapes/REVIEW.md) | [PROGRESS](avatar_modeling/v044_pose_shapes/PROGRESS.md) |
| v045 | [기존 코트의 겹친 정점을 연결](avatar_modeling/v045_coat_connections/REVIEW.md) | [PROGRESS](avatar_modeling/v045_coat_connections/PROGRESS.md) · [SOURCE CORRECTION](avatar_modeling/v045_coat_connections/SOURCE_CORRECTION.md) |
| v046 | [손가락 관절과 손바닥 연결 보정](avatar_modeling/v046_finger_topology/REVIEW.md) | [FIT NOTES](avatar_modeling/v046_finger_topology/FIT_NOTES.md) · [PROGRESS](avatar_modeling/v046_finger_topology/PROGRESS.md) |
| v047 | [어깨 지지 본의 분담 시험](avatar_modeling/v047_coat_support/REVIEW.md) | [PROGRESS](avatar_modeling/v047_coat_support/PROGRESS.md) |
| v048 | [Corrective Smooth와 이중 쿼터니언 비교](avatar_modeling/v048_clothing_correctives/REVIEW.md) | [PROGRESS](avatar_modeling/v048_clothing_correctives/PROGRESS.md) |
| v049 | [기존 어깨 면에 관절 줄 추가](avatar_modeling/v049_arm_loops/REVIEW.md) | [PROGRESS](avatar_modeling/v049_arm_loops/PROGRESS.md) |
| v050 | [어깨 웨이트의 제한 최적화](avatar_modeling/v050_shoulder_fit/REVIEW.md) | [PROGRESS](avatar_modeling/v050_shoulder_fit/PROGRESS.md) |
| v051 | [노멀과 연결 상태의 분리 진단](avatar_modeling/v051_pose_normals/REVIEW.md) | [PROGRESS](avatar_modeling/v051_pose_normals/PROGRESS.md) |
| v052 | [팔 방향에 따른 기존 코트 자세 보정](avatar_modeling/v052_pose_clothing/REVIEW.md) | [PROGRESS](avatar_modeling/v052_pose_clothing/PROGRESS.md) |
| v053 | [코트·손 연결과 자동 노멀 비교](avatar_modeling/v053_surface_finish/REVIEW.md) | — |
| v054 | [뒤 카라의 작은 표면 정리](avatar_modeling/v054_collar_local/REVIEW.md) | — |
| v055 | [통합 사본과 동작 중간값 검사](avatar_modeling/v055_upper_delivery/REVIEW.md) | — |
| v056 | [중간값 회귀를 줄인 옷 자세 보정의 한계](avatar_modeling/v056_upper_refined/REVIEW.md) | — |
| v057 | [여러 자세를 고려한 기존 정점의 기본 위치 조정](avatar_modeling/v057_coat_rest_fit/REVIEW.md) | — |
| v058 | [실제 쇄골–상완 연결에 좁게 적용한 절반 회전](avatar_modeling/v058_shoulder_volume/REVIEW.md) | — |
| v059 | [검증된 손·코트 연결·카라 수정의 선별 사본](avatar_modeling/v059_upper_review/REVIEW.md) | — |
| v060 | [엄지 벌림·맞섬과 손목 범위 추가 검증](avatar_modeling/v060_hand_range/REVIEW.md) | — |
| v061 | [소매 끝의 손 본 영향과 접촉을 함께 비교](avatar_modeling/v061_cuff_weights/REVIEW.md) | — |
| v062 | [소매·팔꿈치 기존 사각형의 대각선 고정](avatar_modeling/v062_sleeve_diagonals/REVIEW.md) | — |
| v063 | [소매의 제한된 웨이트 최적화와 미채택 판정](avatar_modeling/v063_sleeve_fit/REVIEW.md) | — |
| v064 | [뒷겨드랑이의 오목한 부분만 바깥으로 보정](avatar_modeling/v064_shoulder_concavity/REVIEW.md) | — |
| v065 | [고정 삼각 분할과 실제 내보내기 조건의 차이](avatar_modeling/v065_frozen_triangles/REVIEW.md) | — |
| v066 | [팔·몸통 사이 웨이트 분배를 넓게 다시 설정한 비교](avatar_modeling/v066_shoulder_profile/REVIEW.md) | — |
| v067 | [손 노멀만 완만하게 섞은 비교](avatar_modeling/v067_hand_normals/REVIEW.md) | — |
| v068 | [옷·팔·손 작업 종합 검수 — 이 문서부터 보기](avatar_modeling/v068_upper_delivery/START_HERE.md) | [상세 검토](avatar_modeling/v068_upper_delivery/REVIEW.md) · [RUNTIME HANDOFF](avatar_modeling/v068_upper_delivery/RUNTIME_HANDOFF.md) |
| v069 | [손가락 뿌리 MCP 부피 보조본 비교](avatar_modeling/v069_hand_mcp_volume/REVIEW.md) | — |
| v070 | [어깨 접힘의 면·웨이트·회전 원인 재검사](avatar_modeling/v070_upper_cause/REVIEW.md) | — |
| v071 | [기존 표면의 모서리·회전을 보존하는 어깨 목표 시험](avatar_modeling/v071_surface_strain/REVIEW.md) | — |
| v072 | [앞쪽 어깨 보정을 좁은 회전 범위에서 작동시킨 실험](avatar_modeling/v072_compact_front/REVIEW.md) | — |
| v073 | [손의 마지막 면적 경고를 국소 웨이트로 정리](avatar_modeling/v073_hand_residual/REVIEW.md) | — |
| v074 | [어깨의 인접 웨이트 차이를 완만하게 만든 비교](avatar_modeling/v074_shoulder_graph/REVIEW.md) | — |
| v075 | [코트 앞허리의 잔여 바지 접촉 재검사](avatar_modeling/v075_coat_clearance/REVIEW.md) | — |
| v076 | [손의 면적 가중 노멀 비교](avatar_modeling/v076_hand_surface/REVIEW.md) | — |
| v077 | [어깨 기존 사각면의 대각선 선별](avatar_modeling/v077_shoulder_diagonals/REVIEW.md) | — |
| v078 | [중립 소매의 과한 요철을 기존 정점에서 정리](avatar_modeling/v078_sleeve_rest/REVIEW.md) | — |
| v079 | [카라·목 복합 회전과 코트 자락 범위 재검수](avatar_modeling/v079_collar_tail_review/REVIEW.md) | — |
| v080 | [뒤 목의 목–머리 웨이트 국소 보정](avatar_modeling/v080_neck_weights/REVIEW.md) | — |
| v081 | [선별한 손·목 수정의 전신·손목 회귀 검사](avatar_modeling/v081_continued_review/REVIEW.md) | — |
| v082 | [어깨 관절 중심의 안팎·앞뒤 위치 비교](avatar_modeling/v082_shoulder_pivot/REVIEW.md) | — |
| v083 | [1~3번 연속 작업을 한 번에 확인하기](avatar_modeling/v083_review_delivery/START_HERE.md) | [상세 검토](avatar_modeling/v083_review_delivery/REVIEW.md) |
| v084 | [기존 사각면 사이의 경계선 회전](avatar_modeling/v084_armhole_flow/REVIEW.md) | — |
| v085 | [깊은 주름의 실제 정점 추적과 세 꼭짓점 보정](avatar_modeling/v085_fold_landmarks/REVIEW.md) | — |
| v086 | [겨드랑이 접힘 꼭짓점의 국소 정리](avatar_modeling/v086_fold_cleanup/REVIEW.md) | — |
| v087 | [뒤 목 잔여 한 면의 작은 웨이트 보정](avatar_modeling/v087_neck_residual/REVIEW.md) | — |
| v088 | [면 재연결에서 UV 경계를 놓치면 생기는 문제](avatar_modeling/v088_fold_tessellation/REVIEW.md) | — |
| v089 | [이번 연속 개선 시도와 최신 검수 안내](avatar_modeling/v089_review_delivery/START_HERE.md) | [상세 검토](avatar_modeling/v089_review_delivery/REVIEW.md) |
| v090 | [기존 코트 색의 국소 UV 재베이크](avatar_modeling/v090_patch_rebake/REVIEW.md) | — |
| v091 | [UV 제약을 분리한 겨드랑이 면 연결](avatar_modeling/v091_patch_flow/REVIEW.md) | — |
| v092 | [경고 최소 분할의 형상 손실 확인](avatar_modeling/v092_patch_guard/REVIEW.md) | — |
| v093 | [v093 — 이번 텍스처·겨드랑이 후속 작업 안내](avatar_modeling/v093_bake_projection/START_HERE.md) | [상세 검토](avatar_modeling/v093_bake_projection/REVIEW.md) |

## v094–v119 · 다자세 표면 보정과 옷 늘어남

| 단계 | 먼저 읽을 문서 | 추가 자료 |
|---|---|---|
| v094 | [어깨·겨드랑이 변형 재조사와 수정 설계](avatar_modeling/v094_shoulder_research/RESEARCH.md) | — |
| v095 | [단면 전체 웨이트 비교](avatar_modeling/v095_section_weights/REVIEW.md) | — |
| v096 | [높은 bank 웨이트 비교](avatar_modeling/v096_bank_weights/REVIEW.md) | [당시 계획](avatar_modeling/v096_bank_weights/PLAN.md) |
| v097 | [v097 조사와 수정 실험을 한 번에 보기](avatar_modeling/v097_local_pose_shape/START_HERE.md) | [당시 계획](avatar_modeling/v097_local_pose_shape/PLAN.md) · [상세 검토](avatar_modeling/v097_local_pose_shape/REVIEW.md) |
| v098 | [주름 끝에서 소매 뿌리 전체로 수정 범위 변경](avatar_modeling/v098_surface_target/REVIEW.md) | [당시 계획](avatar_modeling/v098_surface_target/PLAN.md) |
| v099 | [복수 자세에서 면을 지키는 표면 보정](avatar_modeling/v099_surface_guard/REVIEW.md) | [당시 계획](avatar_modeling/v099_surface_guard/PLAN.md) |
| v100 | [왼쪽 표면과 접촉 제약](avatar_modeling/v100_surface_contact/REVIEW.md) | [당시 계획](avatar_modeling/v100_surface_contact/PLAN.md) |
| v101 | [오른쪽 소매의 독립 보정](avatar_modeling/v101_right_surface/REVIEW.md) | [당시 계획](avatar_modeling/v101_right_surface/PLAN.md) |
| v102 | [국소 삼각형 사이 분리 축 검사](avatar_modeling/v102_bilateral_guard/REVIEW.md) | [당시 계획](avatar_modeling/v102_bilateral_guard/PLAN.md) |
| v103 | [양쪽 통합, 경계 사각면과 회전 길이 보정](avatar_modeling/v103_bilateral_review/REVIEW.md) | [당시 계획](avatar_modeling/v103_bilateral_review/PLAN.md) |
| v104 | [수정 범위 밖 고정 면까지 접촉 조건 확장](avatar_modeling/v104_right_contact/REVIEW.md) | [당시 계획](avatar_modeling/v104_right_contact/PLAN.md) |
| v105 | [양쪽 함몰 완화와 새로운 회전 검사](avatar_modeling/v105_surface_review/REVIEW.md) | [당시 계획](avatar_modeling/v105_surface_review/PLAN.md) |
| v106 | [늘어남을 제한한 양쪽 소매 보정](avatar_modeling/v106_strain_margin/REVIEW.md) | [METHOD](avatar_modeling/v106_strain_margin/METHOD.md) · [당시 계획](avatar_modeling/v106_strain_margin/PLAN.md) |
| v107 | [앞으로 팔을 들 때의 표면 보정과 음영 분리](avatar_modeling/v107_front_surface/REVIEW.md) | [당시 계획](avatar_modeling/v107_front_surface/PLAN.md) |
| v108 | [이번 연속 수정 결과를 한 번에 확인하기](avatar_modeling/v108_review_delivery/START_HERE.md) | [당시 계획](avatar_modeling/v108_review_delivery/PLAN.md) |
| v109 | [코트 전체의 늘어남 진단](avatar_modeling/v109_garment_strain/REVIEW.md) | [조사·계획](avatar_modeling/v109_garment_strain/RESEARCH_PLAN.md) |
| v110 | [원단 늘어남을 줄이는 넓은 보정 6종](avatar_modeling/v110_cloth_metric/REVIEW.md) | — |
| v111 | [넓은 보정과 충돌 구간 보호](avatar_modeling/v111_cloth_guard/REVIEW.md) | — |
| v112 | [넓은 옷 보정의 자동 포즈 활성 시험](avatar_modeling/v112_garment_pose/REVIEW.md) | — |
| v113 | [중간 회전에서 생긴 오류를 다시 제한](avatar_modeling/v113_garment_range/REVIEW.md) | — |
| v114 | [코트 몸판·자락 전체 들림 시험](avatar_modeling/v114_garment_lift/REVIEW.md) | — |
| v115 | [앞/옆 통합 후보의 새 표본 실패](avatar_modeling/v115_garment_review/START_HERE.md) | — |
| v116 | [옆들기만 선별한 뒤 새 자세 재검사](avatar_modeling/v116_garment_review/START_HERE.md) | — |
| v117 | [복합 몸통 회전에서 드러난 보정 범위 한계](avatar_modeling/v117_garment_review/START_HERE.md) | [당시 후속 과제](avatar_modeling/v117_garment_review/NEXT_WORK.md) |
| v118 | [몸통 조건을 포함한 포즈 보정](avatar_modeling/v118_garment_review/START_HERE.md) | [당시 후속 과제](avatar_modeling/v118_garment_review/NEXT_WORK.md) |
| v119 | [옷의 늘어남 후속 작업 종합 검수](avatar_modeling/v119_garment_review/START_HERE.md) | [당시 후속 과제](avatar_modeling/v119_garment_review/NEXT_WORK.md) |

## v120–v152 · 앞뒤 회전·어깨선·두께

| 단계 | 먼저 읽을 문서 | 추가 자료 |
|---|---|---|
| v120 | [앞뒤 회전 경로 진단](avatar_modeling/v120_front_axis/REVIEW.md) | [조사·계획](avatar_modeling/v120_front_axis/RESEARCH_PLAN.md) |
| v121 | [팔 앞뒤 회전축 검수](avatar_modeling/v121_arm_controls/START_HERE.md) | — |
| v122 | [앞들기 수축 보간 진단](avatar_modeling/v122_forward_surface/REVIEW.md) | [당시 계획](avatar_modeling/v122_forward_surface/PLAN.md) |
| v123 | [기존 코트의 자기 충돌 포함 물리 목표 실험](avatar_modeling/v123_cloth_targets/REVIEW.md) | — |
| v124 | [원단 계산에서 얻은 앞들기 보정 목표 선별](avatar_modeling/v124_front_guard/REVIEW.md) | — |
| v125 | [앞들기 자동 보정 시험, 통합 보류](avatar_modeling/v125_front_pose/REVIEW.md) | — |
| v126 | [앞으로 드는 팔의 관절 중심·쇄골·웨이트 점검](avatar_modeling/v126_shoulder_audit/START_HERE.md) | — |
| v127 | [관절 깊이와 코트 웨이트 결합 시험](avatar_modeling/v127_shoulder_trials/REVIEW.md) | — |
| v128 | [기존 옆·뒤 표면을 목표로 한 웨이트 적합](avatar_modeling/v128_shoulder_fit/REVIEW.md) | — |
| v129 | [앞들기에서만 적용하는 코트 경계 보정](avatar_modeling/v129_front_boundary/REVIEW.md) | — |
| v130 | [앞들기 경계 보정의 회귀 면 보호](avatar_modeling/v130_boundary_guard/REVIEW.md) | — |
| v131 | [저장 모션에서 추가 회귀 발견](avatar_modeling/v131_shoulder_review/REVIEW.md) | — |
| v132 | [어깨 전진 동작 수정과 코트 경계 보정 검수](avatar_modeling/v132_shoulder_review/START_HERE.md) | — |
| v133 | [표면 세부 운반 목표 6종](avatar_modeling/v133_surface_transport/REVIEW.md) | [조사·계획](avatar_modeling/v133_surface_transport/RESEARCH_PLAN.md) |
| v134 | [앞들기 표면 보정의 적용 범위 제한](avatar_modeling/v134_transport_keys/REVIEW.md) | — |
| v135 | [소매 찌그러짐 추가 보정 검수](avatar_modeling/v135_transport_review/START_HERE.md) | [당시 후속 과제](avatar_modeling/v135_transport_review/NEXT_WORK.md) |
| v136 | [기본/앞/뒤 어깨 폭 진단](avatar_modeling/v136_shoulder_width/REVIEW.md) | — |
| v137 | [앞90/뒤35 폭 보정과 적용 제한](avatar_modeling/v137_width_correction/REVIEW.md) | — |
| v138 | [앞뒤 팔 동작의 어깨 폭 비교·수정](avatar_modeling/v138_width_review/START_HERE.md) | [추가 시점 비교](avatar_modeling/v138_width_review/extra_views/README.md) |
| v139 | [팔 앞뒤 동작의 끊긴 선·텍스처·음영 원인 진단](avatar_modeling/v139_seam_diagnosis/START_HERE.md) | — |
| v140 | [어깨의 각진 음영 정리](avatar_modeling/v140_shading_fix/REVIEW.md) | — |
| v141 | [어깨에 그려진 검은 띠 국소 보정](avatar_modeling/v141_texture_fix/REVIEW.md) | — |
| v142 | [겨드랑이 압축 면 국소 복원 실험](avatar_modeling/v142_fold_trials/REVIEW.md) | — |
| v143 | [텍스처 UV 여백 보완 비교안](avatar_modeling/v143_seam_review/START_HERE.md) | — |
| v144 | [정장 어깨선을 유지하면서 변형 원인 확인](avatar_modeling/v144_seam_transport/START_HERE.md) | — |
| v145 | [v145은 중간 실험 — 최신 검수는 v146](avatar_modeling/v145_seam_review/START_HERE.md) | — |
| v146 | [정장 어깨선 보존·변형·대각선 검수](avatar_modeling/v146_seam_topology/START_HERE.md) | — |
| v147 | [팔 전방 거상에서 어깨가 납작해지는 원인](avatar_modeling/v147_shoulder_volume/START_HERE.md) | — |
| v148 | [소매산 두께와 회전 억제 비교 실험](avatar_modeling/v148_volume_trials/START_HERE.md) | — |
| v149 | [연속 두께 복원과 접촉 제한 실험](avatar_modeling/v149_volume_fit/START_HERE.md) | — |
| v150 | [면 고정 방식의 검증과 미채택](avatar_modeling/v150_volume_review/START_HERE.md) | — |
| v151 | [부피 복원 후보의 중간 검증](avatar_modeling/v151_volume_review/START_HERE.md) | — |
| v152 | [팔을 앞으로 뻗을 때 어깨 두께 복원](avatar_modeling/v152_volume_review/START_HERE.md) | — |

## v153–v165 · 참고 모델 비교와 카라·헤어·소매

| 단계 | 먼저 읽을 문서 | 추가 자료 |
|---|---|---|
| v153 | [Lapwing 구매 아바타 구조 조사](avatar_modeling/v153_lapwing_reference/START_HERE.md) | — |
| v154 | [어깨 지지 본·단계 회전 비교, 미채택](avatar_modeling/v154_shoulder_support/START_HERE.md) | — |
| v155 | [다자세 웨이트 맞춤, 미채택](avatar_modeling/v155_weight_fit/START_HERE.md) | — |
| v156 | [비앙카·Lapwing·SiuSiu 구조 비교](avatar_modeling/v156_three_avatar_comparison/START_HERE.md) | — |
| v157 | [Lapwing·SiuSiu 관절을 실제로 움직여 비교](avatar_modeling/v157_reference_pose/START_HERE.md) | — |
| v158 | [카라·헤어·의상 물리 조사와 적용 계획](avatar_modeling/v158_dynamics_research/RESEARCH.md) | — |
| v159 | [카라·라펠이 팔에 끌리는 현상 완화](avatar_modeling/v159_collar_support/START_HERE.md) | — |
| v160 | [원래 헤어 형태를 보존하는 PhysBone 준비와 실제 Unity 검사](avatar_modeling/v160_hair_dynamics/START_HERE.md) | — |
| v161 | [카라·헤어 통합 및 코트 객체 분리](avatar_modeling/v161_separate_coat/START_HERE.md) | — |
| v162 | [전완 Twist 분산 실험, 통합 보류](avatar_modeling/v162_forearm_twist/START_HERE.md) | — |
| v163 | [비앙카 v163 — 이번 작업 한 번에 확인하기](avatar_modeling/v163_review_bundle/START_HERE.md) | — |
| v164 | [비앙카 관절·의상·Unity 통합 조사와 실행 계획](avatar_modeling/v164_integration_research/RESEARCH_AND_PLAN.md) | — |
| v165 | [손목과 정장 소매 끝의 회전 역할 분리](avatar_modeling/v165_cuff_roles/START_HERE.md) | — |

## v166–v180 · Unity 자동 보정·물리·Humanoid 통합

| 단계 | 먼저 읽을 문서 | 추가 자료 |
|---|---|---|
| v166 | [어깨 국소 변형 실험, 아직 미채택](avatar_modeling/v166_shoulder_local/START_HERE.md) | — |
| v167 | [Unity PC 통합·실제 SDK 검증 작업 중](avatar_modeling/v167_unity_integration/START_HERE.md) | — |
| v168 | [고정 삼각면 비교, 아직 미채택](avatar_modeling/v168_fixed_coat/START_HERE.md) | [당시 계획](avatar_modeling/v168_fixed_coat/PLAN.md) |
| v169 | [원래 오목한 주름을 채우는 국소 실험, 미채택](avatar_modeling/v169_rest_fold/START_HERE.md) | [당시 계획](avatar_modeling/v169_rest_fold/PLAN.md) |
| v170 | [자동 보정·바람·Humanoid 통합 검사 진행](avatar_modeling/v170_runtime_validation/START_HERE.md) | [성능 비교](avatar_modeling/v170_runtime_validation/PERFORMANCE_COMPARISON.md) · [당시 계획](avatar_modeling/v170_runtime_validation/PLAN.md) |
| v171 | [기존 어깨 면 연결 회전, 별도 실험](avatar_modeling/v171_edge_flow/START_HERE.md) | [당시 계획](avatar_modeling/v171_edge_flow/PLAN.md) |
| v172 | [기존 두 면 연결만 수정한 후보](avatar_modeling/v172_local_topology/START_HERE.md) | [당시 계획](avatar_modeling/v172_local_topology/PLAN.md) |
| v173 | [자동 보정 계산 단계 축소](avatar_modeling/v173_fast_correctives/START_HERE.md) | [당시 계획](avatar_modeling/v173_fast_correctives/PLAN.md) |
| v174 | [연속 통합 시험과 새로 발견한 결함](avatar_modeling/v174_continuous_qa/START_HERE.md) | [당시 계획](avatar_modeling/v174_continuous_qa/PLAN.md) |
| v175 | [실제 Humanoid 기준 자세·손가락 매핑 수정](avatar_modeling/v175_humanoid_reference/START_HERE.md) | [당시 계획](avatar_modeling/v175_humanoid_reference/PLAN.md) |
| v176 | [검증된 코트 연결을 Unity에 반영](avatar_modeling/v176_unity_surface/START_HERE.md) | [당시 계획](avatar_modeling/v176_unity_surface/PLAN.md) |
| v177 | [lilToon Soft 검토 재질 채택](avatar_modeling/v177_material_review/START_HERE.md) | [당시 계획](avatar_modeling/v177_material_review/PLAN.md) |
| v178 | [동작 중 오목한 홈 완화, 세 후보 미채택](avatar_modeling/v178_pose_fold/START_HERE.md) | [당시 계획](avatar_modeling/v178_pose_fold/PLAN.md) |
| v179 | [기존 지지선 추가 시험, 미채택](avatar_modeling/v179_joint_support_cuts/START_HERE.md) | [당시 계획](avatar_modeling/v179_joint_support_cuts/PLAN.md) |
| v180 | [비앙카 통합 검토 v180](avatar_modeling/v180_integrated_review/START_HERE.md) | [당시 계획](avatar_modeling/v180_integrated_review/PLAN.md) |

## v181–v186 · 잔여 경고 분류와 국소 면 검토

| 단계 | 먼저 읽을 문서 | 추가 자료 |
|---|---|---|
| v181 | [v181 미해결 경고의 부위 분류](avatar_modeling/v181_joint_localization/START_HERE.md) | [당시 계획](avatar_modeling/v181_joint_localization/PLAN.md) |
| v182 | [v182 골반 지지 후보 — 미채택](avatar_modeling/v182_pelvis_weights/START_HERE.md) | [당시 계획](avatar_modeling/v182_pelvis_weights/PLAN.md) |
| v183 | [v183 어깨 국소 웨이트 최적화 — 미채택](avatar_modeling/v183_shoulder_local_weights/START_HERE.md) | [당시 계획](avatar_modeling/v183_shoulder_local_weights/PLAN.md) |
| v184 | [v184 기존 면의 삼각 분할 수정 — 최종10곳 반영](avatar_modeling/v184_body_diagonals/START_HERE.md) | [당시 계획](avatar_modeling/v184_body_diagonals/PLAN.md) |
| v185 | [v185 인접 손 삼각면 연결 후보 — 미채택](avatar_modeling/v185_web_edgeflow/START_HERE.md) | [당시 계획](avatar_modeling/v185_web_edgeflow/PLAN.md) |
| v186 | [이번 개선을 한 번에 확인](avatar_modeling/v186_review_bundle/START_HERE.md) | — |

## 별도 초기 조사

[초기 제작 워크플로 조사](docs/NANO_WORKFLOW_RESEARCH.md)는 당시 검토한 선택지를 담은 역사 기록입니다. 현재 실행 방침과 구분해 읽어 주세요.

[README로 돌아가기](README.md) · [상태·결정 이력](STATUS.md) · [공유 범위](SHARING.md)

환경 준비 보충: [Mac용 VRChat SDK 프로젝트](docs/VRCHAT_MAC_SETUP.md). 모델 단계와 별도인 설치·첫 실행 기록입니다.

## v257 · Mac 재현 검증 보충

| 단계 | 먼저 읽을 문서 | 상태 |
|---|---|---|
| v257 | [v257 Mac 재현 검증](avatar_modeling/v257_mac_reproduction/START_HERE.md) | 환경·재현 완료, 관절 수정 완료 아님 |

v187–v256 모델 단계 전체를 이번 보충에 포함했다는 뜻은 아닙니다.


## v273·v275 · 근본 원인 수정과 회귀 검증 보충

| 단계 | 먼저 읽을 문서 | 상태 |
|---|---|---|
| v273 | [원래 코트 겹면 분리](avatar_modeling/v273_neutral_layer_repair/START_HERE.md) | 정지 자세 통과, 동작 회귀로 미채택 |
| v275 | [정지 수정의 동작 재검증](avatar_modeling/v275_rest_repair_validation/START_HERE.md) | 회귀를 확인해 미채택 |

v258–v274 전체 기록이나 v276 이후 진행 중 작업을 포함한 것은 아닙니다.

## Mac 선별 보충 · v295

| 단계 | 먼저 읽을 문서 | 상태 |
|---|---|---|
| v295 | [v295 소매 부피와 실제 스키닝 검수](avatar_modeling/v295_pose_target_skinning/README.md) |20자세 명시적 키 스키닝 검증, 자동 전환·접촉 미완료 |


## Mac 선별 보충 · v308

| 단계 | 먼저 읽을 문서 | 상태 |
|---|---|---|
| v308 | [부피 계산 수정과 형상 재검수](avatar_modeling/v308_smooth_local_contact/README.md) | 계산 원인 분리·기본 두께 확인, 복합 접힘/접촉 미해결·미채택 |


## Mac 선별 보충 · v315–v318

[v318 이번 작업 확인 문서](v318_WORK_REVIEW.md)에서 전후 사진, 부피·늘어남 검사, 미채택 이유와 남은 작업을 한 번에 확인할 수 있습니다. 전체 아바타 완료본은 아닙니다.

| 단계 | 먼저 읽을 문서 | 상태 |
|---|---|---|
| v315 | [원형 주름과 두께 키 분리](avatar_modeling/v315_volume_key_audit/README.md) | 원래 주름/새 변형 분리, 미채택 |
| v316 | [부피와 면 길이 검수](avatar_modeling/v316_volume_shape_balance/README.md) | 늘어남 감소·셔츠 접촉 회귀, 미채택 |
| v318 | [안쪽 셔츠 분리 조건](avatar_modeling/v318_inner_shirt_clearance/README.md) | 선택 제약 불성립, 모델 미적용 |

## Mac 텍스처·전체 관절 검수 후속 · v319–v336 뒤트임의 새 밝은 점선으로 미채택, 현재 사용 기준은 v334입니다.

| 단계 | 보고서 | 상태 |
|---|---|---|
| v319 | [문제·시도·미채택](avatar_modeling/v319_issue_review/v319_ISSUES_AND_ATTEMPTS.md) | 사진③ 우선 유지 |
| v320 | [완전 신전 사진](avatar_modeling/v320_straight_arm/README.md) | 실제 자세 비교 |
| v321 | [측면 곡선 비교](avatar_modeling/v321_arm_curve_audit/README.md) | 사용자 첨부 수준 허용 |
| v322 | [텍스처 재작업](avatar_modeling/v322_texture_rework/v322_TEXTURE_REVIEW.md) | 기존 UV 정리 후보 |
| v323 | [숨은 표면·새 UV](avatar_modeling/v323_hidden_texture_uv/v323_TEXTURE_UV_REVIEW.md) | 분리 전후 검수, 새 UV 비용·늘어짐 명시한 후보 |
| v324 | [기존 디테일 복원](avatar_modeling/v324_detail_restore/v324_DETAIL_REVIEW.md) | v323 평탄화 수정, 기존 명암·UV 밀도 복원 검수본 |
| v325 | [전체 관절·본 검토](avatar_modeling/v325_full_rig_audit/v325_RIG_REVIEW.md) | 129본·300자세·41장, 원본 보존·변형 품질 미완료 |
| v326 | [팔꿈치 정면 각짐](avatar_modeling/v326_elbow_front_audit/v326_ELBOW_REVIEW.md) | 실루엣·기하 확인, 국소 검토 재개·모델 미수정 |
| v327 | [팔꿈치 정면 각짐 수정](avatar_modeling/v327_elbow_fix/v327_ELBOW_FIX_REVIEW.md) | 원형 보존·자동 보정2키·23정적/181연속, 자기 교차 잔여·별도 후보 |
| v328 | [참고 모델의 강한 모으기](avatar_modeling/v328_reference_elbow/v328_REFERENCE_REVIEW.md) |3모델·방향 일치 읽기 전용 비교, v327 측면 채택 보류 |
| v329 | [SiuSiu 기준 차이 조사](avatar_modeling/v329_siusiu_elbow/v329_SIUSIU_COMPARISON.md) |12자세·비례/웨이트/부피, 모델 수정 없음·v327 보류 유지 |
| v330 | [측면 유지 팔꿈치·3열 사진 비교](avatar_modeling/v330_elbow_profile/v330_ELBOW_COMPARISON.md) | 기존36정점·작은 보정2키, 원형 보존·비틀림 교차 잔여인 검수 후보 |
| v331 | [팔꿈치 세 이슈 위치·사진](avatar_modeling/v331_elbow_issues/v331_ISSUE_ATLAS.md) | 위치 표시7사진·진단2그림, 읽기 전용·v330 후보 유지 |
| v332 | [SiuSiu 셰이더 분석·작업 계획](avatar_modeling/v332_shader_research/v332_SHADER_REVIEW.md) | 원본10재질·기능 OFF 사진8그림, 비앙카 제작·적용 전 |
| v332 | [팔꿈치 ②·③ 분리 보정](avatar_modeling/v332_elbow_fold/v332_FOLD_REVIEW.md) | 정면·모으기4키, 14사진·실제205기록, 부분 개선 후보·① 보류 |

| v333 | [팔꿈치 하단 패임·3버전 비교](avatar_modeling/v333_elbow_contour/v333_CONTOUR_REVIEW.md) | 새2키·24사진·미채택6사진, 기존형상/부피검사·작은각잔여 |

| v334 | [오른쪽 팔꿈치 끝 미세 조정](avatar_modeling/v334_elbow_tip/v334_TIP_REVIEW.md) | 기존13점·16사진, 작은기울기변경·검수후보 |

| v335 | [UV·텍스처 비교](avatar_modeling/v335_uv_texture_audit/v335_UV_TEXTURE_REVIEW.md) | 3모델·20그림, 국소 UV 전사 계획·모델수정0 |

| v336 | [디테일 보존 UV·텍스처 재작업](avatar_modeling/v336_uv_detail_rework/v336_UV_TEXTURE_REVIEW.md) | 새UV/4K전사 후보·원래디테일보존·잔여왜곡 기록 |
| v337 | [채색용 UV 작업본](avatar_modeling/v337_paintable_uv/v337_UV_WORK_REVIEW.md) | 541패널·작은204개·디테일 보존·마스크/사진 검수 |
| v338 | [국소 텍스처 1차 검수](avatar_modeling/v338_local_texture/v338_TEXTURE_REVIEW.md) | 허벅지 손 흔적·신발 반점 정리, 셔츠 시험 미반영·후속 잔여 |
| v339 | [선·명암 경계 정리](avatar_modeling/v339_texture_clarity/v339_CLARITY_REVIEW.md) | 셔츠·소매·신발 일부, 3강도·미채택 구간 비교 |
| v340 | [소매 단추 정렬·디테일](avatar_modeling/v340_cuff_buttons/v340_BUTTON_REVIEW.md) | 원래 8개 형상 유지, 전용 UV·봉제 디테일·잔상 정리 |
| v341 | [전체 텍스처·Head UV](avatar_modeling/v341_full_texture_detail/v341_TEXTURE_REVIEW.md) | 부위별 Head 배치·국소 디테일·넥타이 분류/색 수정 |
| v342 | [텍스처 번짐·아티팩트 조사](avatar_modeling/v342_texture_artifact_audit/v342_ARTIFACT_LIST.md) | 수정 후보 14·보류 3, 위치·확대 사진과 숨은 면 검사; 수정 없음 |

| v343 | [텍스처·UV 재작업](avatar_modeling/v343_texture_resolution/v343_WORK_REVIEW.md) | Head 재전개·다방향 채색·디테일 보존·실제 전후 |
| v344 | [전 관절·129본·움직임 GIF](avatar_modeling/v344_texture_rig_retest/v344_WORK_REVIEW.md) | 정적299·연속121·물리360프레임, 기존 접촉 잔여 |

| v345 | [NPR 계획·독립 검토 절차](avatar_modeling/v345_npr_work_plan/v345_WORK_PLAN.md) | 계획만 작성, 제작·에이전트 실행 보류 |
| v346 | [NPR 텍스처 재작업](avatar_modeling/v346_npr_texture/v346_WORK_REVIEW.md) | 헤드·안쪽·신발, 전사 원인 수정·디테일 보존 |
| v347 | [헤어·넥타이 실제 후보 비교](avatar_modeling/v347_secondary_motion/v347_WORK_REVIEW.md) | 물리·웨이트·바람 분리, 미채택 시도 |
| v348 | [NPR 통합·129본·관절·영상](avatar_modeling/v348_final_npr_review/v348_WORK_REVIEW.md) | 새 셔츠 교차 수정·실제 재검사, 잔여 범위 기록 |

## 셰이더 별도 작업

사용자 요청으로 `avatar_shader`에서 별도 번호를 사용합니다. 위 모델링 버전과 독립된 단계입니다.

| 단계 | 보고서 | 상태 |
|---|---|---|
| 셰이더 v001 | [SiuSiu 방식 실제 시험](avatar_shader/v001_siusiu_style/v001_SHADER_REVIEW.md) | B/C/D/E 재질·패키지, 강한광택제외·E잔여있는검수후보 |

셰이더 v001은 [질감 표현 목표 정정](avatar_shader/v001_siusiu_style/v001_FEEDBACK.md)을 먼저 읽는다. 기존 시험 기록은 보존한다.

- [셰이더 v002 · 소재별 대응 조사](avatar_shader/v002_material_mapping/v002_MATERIAL_REVIEW.md): 비앙카 디자인과 소재 구분을 우선, SiuSiu 반사 일괄이식 보류. 기존 사진 재검토·새 편집 없음.

- [셰이더 v003 · 머리카락·피부·눈 실제 4후보](avatar_shader/v003_surface_candidates/v003_SURFACE_REVIEW.md): 69렌더/14비교그림, 부위별 비교·잔여문제 포함. 네후보 최종미채택·VRChat미검증.

- [셰이더 v004 · 고정 그림자와 2D 대안](avatar_shader/v004_texture_shadow_comparison/v004_WORK_REVIEW.md): A/B/C와 SiuSiu, 광원별63렌더·고정 명암 감소 사본 비교.

- [셰이더 v005 · NPR 명암/노멀 A/B/C/D](avatar_shader/v005_npr_roles/v005_WORK_REVIEW.md): 186 실제 렌더, D 통합 후보·B/C 미채택·SiuSiu 참고.

# 문제별 작업 추적

2026-09-12 · [v322 텍스처 재작업·UV 경계 정리](avatar_modeling/v322_texture_rework/v322_TEXTURE_REVIEW.md): 코트·바지·셔츠 재채색, 투영 색 혼입과 패딩 덮어쓰기 수정. 원형·UV 좌표 보존, Unity 실제 전후 사진과 미채택 시험을 기록했습니다. 뒤목의 희미한 윤곽과 셔츠 세부는 남아 최종 채색 채택은 별도입니다.

2026-09-12 · [v321 주름 출처와 측면 휘어짐](avatar_modeling/v321_arm_curve_audit/README.md): 같은 자세에서 원형 전완 주름과 최근 윗소매의 추가 꺾임을 구분했습니다. 본은 일직선이나 소매 표면은 실제로 달라졌습니다. 모델 수정 없이 사진③을 보존한 비교입니다.

2026-09-12 · [v320 팔을 완전히 편 정면·측면 비교](avatar_modeling/v320_straight_arm/README.md): 사진③의 어깨 방향을 유지하고 팔꿈치를 약9.23°→0°로 편 사진을 직접 확인했습니다. 임시 기존 메시 사본의 자세 비교이며 전체 자동 구동 완료는 아닙니다.

2026-09-12 · [v319 문제·시도·미채택 사유](avatar_modeling/v319_issue_review/v319_ISSUES_AND_ATTEMPTS.md): Mac 작업의 주요 시도와 제외·보류 이유, 직접 확인한 사진6장, 부피·접촉 수치의 한계와 피드백 항목을 정리했습니다. 문서 정리 단계이며 새 모델 수정은 없습니다. 형상 문제 미해결·후보 미채택 상태로 사용자 피드백을 기다립니다.

이 문서는 같은 문제가 여러 버전에서 어떻게 조사·수정·재검증됐는지 이어 읽기 위한 지도입니다. **공유 경계는 2026-09-11의 v186**입니다. 진행 중인 v187 이후의 결과는 반영하지 않았습니다. 전체 흐름과 비교 그림은 [README](README.md), 모든 공개 보고서는 [STAGES](STAGES.md), 시점별 결정은 [STATUS](STATUS.md)에서 확인할 수 있습니다.

아래 T01–T14는 이 문서의 고정 추적 번호입니다. GitHub 이슈 번호나 자동 진행률이 아닙니다. 각 항목은 **문제 → 출발 기준 → 실험과 판정 → 확인 범위 → 남은 일** 순서로 읽습니다. `반영`은 해당 후속 검토본에 들어갔다는 뜻이며 전체 아바타 완성 승인을 뜻하지 않습니다. `미채택`은 검토 결과를 남겼지만 기준에 합치지 않은 시도입니다.

## 관심 있는 문제에서 시작하기

| 추적 | 문제 | 주요 경로 | v186 공유 시점의 판단 |
|---|---|---|---|
| [T01](#t01) | 얼굴·헤어 색과 눈 정렬 | v001 → v004 → v005 | 원래 형상에 채색을 맞추는 방향 선택 |
| [T02](#t02) | 무릎 접힘과 실루엣 | v030 → v033–v037 → v038 | SMALL 사용자 채택, 기존 안쪽 교차 잔여 |
| [T03](#t03) | 손가락 마디·엄지·손목 | v023 → v046 → v068 → v083 | 선별 반영, 복합 손 자세 미완료 |
| [T04](#t04) | 코트 어깨 홈과 늘어남 | v083 → v089 → v093 → v108 → v119 | 실패안 제외, 옆 들기 보정 제한 반영 |
| [T05](#t05) | 팔 조작 축과 재생 경로 | v119 → v121 → v132 | 축 분리·회전 보간 오류 수정 |
| [T06](#t06) | 어깨선·음영·소매 두께 | v139 → v146 → v152 → v155 | 선·두께 부분 개선, 큰 접힘 잔여 |
| [T07](#t07) | 팔을 들 때 카라 늘어남 | v157 → v159 → v163 | 웨이트 전이 반영, 일부 접촉 잔여 |
| [T08](#t08) | 헤어 물리와 기본 모양 | v160 → v163 → v175 | 완만한 후보 반영, Editor에서 구동 확인 |
| [T09](#t09) | 코트 분리 후 음영 차이 | v161 → v163 | 원래 노멀 정보 복원 후 반영 |
| [T10](#t10) | 손목과 소매 끝 충돌 | v162 → v165 | 소매 회전 분담 반영, 기존 접촉 잔여 |
| [T11](#t11) | Unity 자동 자세 보정 | v167 → v170 → v173 | 실제 SDK 재현·이동/크기 조건 검증 |
| [T12](#t12) | Humanoid·손가락·연속 동작 | v174 → v175 → v180 | 기준 자세·매핑 수정, 2프레임 지연 잔여 |
| [T13](#t13) | 물리·보정의 성능 비용 | v170 → v173 → v180 | 비용 비교 후 기능 유지, 클라이언트 미측정 |
| [T14](#t14) | 몸의 잔여 오류와 전달 검증 | v181 → v182–v185 → v186 | 기존 면 10곳만 선별, 전체 품질 미완료 |

## 출발점부터 결과까지

<a id="t01"></a>
### T01 · 색의 오류와 눈 형상을 분리

**문제:** 얼굴과 헤어에 다른 부위의 색이 묻고 눈의 그림과 입체 표면이 어긋났습니다. 형상을 바꾸기 전에 조명 없는 색과 회색 표면으로 원인을 나눴습니다.

| 순서 | 수행·관찰 | 판정과 다음 연결 |
|---|---|---|
| [v001 원본 진단](avatar_modeling/v001_inspection/REVIEW.md) | 리그 없는 원본의 구조·색·임시 관절을 검사 | 기존 외형을 기준으로 보존 |
| [v004 A/B](avatar_modeling/v004_eyes_ab/REVIEW.md) | A는 기존 눈에 채색 정렬, B는 눈 정점을 이동 | B의 이동만으로 색 오류가 해결되지 않음. 사용자 A 방향 선택 |
| [v005 전신 채색](avatar_modeling/v005_texture/REVIEW.md) | 새 채색을 기존 UV로 베이크, 형상·본·웨이트 유지 | 색 번짐 개선. [전후 그림](avatar_modeling/v005_texture/GALLERY.md)으로 외형 차이 확인 |

2026-09-12 · [v323 가려진 표면 텍스처·새 UV](avatar_modeling/v323_hidden_texture_uv/v323_TEXTURE_UV_REVIEW.md): 코트 안쪽·옆다리·손·소매 등 원래 17,659면을 분류해 분리 검수하고 오염색을 정리했습니다. 새 UV 적용·겹침 수정과 원형 보존을 확인했으며, 작은 부품의 UV 늘어짐과 텍스처 비용 증가는 남아 별도 후보입니다. [전체 부위 사진](avatar_modeling/v323_hidden_texture_uv/v323_SURFACE_GALLERY.md)에서 전후를 볼 수 있습니다.

2026-09-12 · [v324 기존 디테일 복원](avatar_modeling/v324_detail_restore/v324_DETAIL_REVIEW.md): 사용자 지적대로 v323에서 신발 장식·옷 주름·머릿결 명암이 과도하게 줄었습니다. 기존 v322 디테일과 UV 밀도를 복원하고 실제 오염 부위만 정리했습니다. 기하·부피·본·웨이트 보존, 3면 UV 이동, Unity UV용 복제점 1,649→10개. [숨은 표면 12그룹 사진](avatar_modeling/v324_detail_restore/v324_SURFACE_GALLERY.md)과 시도·미채택 이유를 함께 기록했습니다. 전체 모델 최종 채택은 아닙니다.

**남은 일:** 기존 눈 조각과 헤어의 입체적 특성은 채색으로 해결하지 않았습니다. 선택한 작업 방식과 최종 채색 완성 승인을 구분합니다.

<a id="t02"></a>
### T02 · 무릎은 v030으로 돌아가 보호할 접힘을 먼저 결정

2026-09-12 · [v326 팔꿈치 정면 각짐](avatar_modeling/v326_elbow_front_audit/v326_ELBOW_REVIEW.md): 측면 허용 뒤 정면 외곽을 재검토했습니다. 무조명 실루엣에서도 각짐이 남으며 보정 키 과적용은 아닙니다. 모델 수정 없이 원인 범위와 후속 국소 비교를 정리했고 무릎·손가락 유지 결정을 보존합니다.

2026-09-12 · [v325 全관절·본 검토](avatar_modeling/v325_full_rig_audit/v325_RIG_REVIEW.md): 현재 v324의 129본·86회전 채널·300정적 자세와 연속121표본을 검토했습니다. 기본 본 연결·웨이트는 통과했으나 어깨·소매, 골반·무릎, 손의 접힘 품질은 미완료입니다. 직접 확인한41장, [129본 점검표](avatar_modeling/v325_full_rig_audit/v325_BONE_TABLE.md), 부피·교차 검사 한계와 실패한 검사 시도를 기록했습니다. 원본 모델은 수정하지 않았습니다.

**문제:** 교차를 줄이려고 구조를 늘릴수록 접히는 틈이 넓어지거나 압축된 주름이 부자연스러워졌습니다.

| 순서 | 수행·관찰 | 판정과 다음 연결 |
|---|---|---|
| [v030 국소 기준](avatar_modeling/v030_joint_checkpoint/REVIEW.md) | 무릎 보조 축과 국소 웨이트 검사 | 이후 외형 비교의 기준으로 보존 |
| [v033 구조](avatar_modeling/v033_knee_structure/REVIEW.md), [v035 두께](avatar_modeling/v035_knee_volume/REVIEW.md), [v036 접힘](avatar_modeling/v036_knee_crease/REVIEW.md) | 컷·본·자세 보정으로 변형 분담 확대 | 검사에서 교차가 줄어도 주름·틈의 외형이 만족스럽지 않아 자동 채택 중단 |
| [v030 재출발 결정](avatar_modeling/v036_knee_crease/RESTART_FROM_V030.md) | 앞선 사본을 누적하지 않고 기준으로 돌아감 | 뒤쪽 접힘을 보존하는 방향으로 범위 재설정 |
| [v038 SMALL](avatar_modeling/v038_knee_front/REVIEW.md) | 기존 64개 정점 웨이트만 변경, 보호 영역 32자세 위치 차이 0 | 사용자 채택. 전신·집중·스트레스·동작에서 새 악화 검사 |

**남은 일:** 안쪽의 기존 교차는 남습니다. 이 경로의 성공 기준은 모든 교차 제거가 아니라 사용자가 선택한 접힘 보존과 앞쪽의 작은 개선입니다. [120도 비교 그림](avatar_modeling/v038_knee_front/DETAIL_120.png)

<a id="t03"></a>
### T03 · 손은 면적만 정상이어도 서로 관통할 수 있음

**문제:** 손가락 본을 추가해도 주먹과 엄지 부근에서 납작해짐·교차가 남았습니다.

| 순서 | 수행·관찰 | 판정과 다음 연결 |
|---|---|---|
| [v023 전신 리그](avatar_modeling/v023_rig_review/REVIEW.md) | 손 82자세의 면적 결과와 교차 결과가 달랐음 | 실제 삼각형 교차를 별도 품질 기준으로 유지 |
| [v046 손 구조](avatar_modeling/v046_finger_topology/REVIEW.md) | 엄지 축·8개 마디 보조 본·9곳 대각선·손 웨이트 조정 | 같은 1,100명령에서 면적 경고 571→3, 교차는 감소했으나 잔여 |
| [v068 선별 검토](avatar_modeling/v068_upper_delivery/START_HERE.md) | 손목 범위와 노멀 등 후속 실험을 모아 비교 | 결과가 나쁜 시도는 빼고 선별. [손 비교](avatar_modeling/v068_upper_delivery/HAND_COMPARE.png) |
| [v083 통합](avatar_modeling/v083_review_delivery/START_HERE.md) | 손 15개 raw 정점과 뒤 목 14개 raw 정점의 국소 웨이트 추가 | 무릎 기준을 보존하면서 제한 반영 |

**남은 일:** 복합 손 자세와 손가락 사이 일부 면은 미완료입니다. 후속 대각선 수정은 [T14](#t14)에서 이어집니다. 축이 바뀐 손 비교는 같은 각도 명령과 같은 공간 자세가 다를 수 있습니다.

<a id="t04"></a>
### T04 · 어깨 홈을 줄이려다 생긴 형상·UV 회귀를 선별

**문제:** 겨드랑이와 어깨를 완화하면 다른 자세에서 늘어나거나 텍스처 전달이 깨졌습니다. 한 사본에서 둘 다 바꾸지 않고 결과를 분리해 판단했습니다.

| 순서 | 수행·관찰 | 판정과 다음 연결 |
|---|---|---|
| [v089 선별](avatar_modeling/v089_review_delivery/START_HERE.md) | v084–v088의 면·웨이트 실험 비교, UV 묶음을 넘는 면 합치기에서 줄무늬 | 실패안 제외. v083에 목 6개 정점의 작은 웨이트 조정만 추가 |
| [v093 색 전달](avatar_modeling/v093_bake_projection/START_HERE.md) | 직접 베이크의 검은 누락을 보완 | 색 개선과 별개로 형상 새 경고가 남아 미채택 |
| [v097 13개 실험](avatar_modeling/v097_local_pose_shape/START_HERE.md) | 웨이트와 국소 자세 키를 분리 시험 | 모두 새 변형 경고가 있어 v089 유지 |
| [v108 통합](avatar_modeling/v108_review_delivery/START_HERE.md) | 양쪽 옆 들기 보정 2개 선별 | 앞 들기 v107은 제외 |
| [v119 넓은 분담](avatar_modeling/v119_garment_review/START_HERE.md) | 심한 방향 늘어남을 주변으로 분담, 몸통 조건으로 작동 제한 | 새 542자세·전신·저장 동작의 해당 검사에서 새 경고 0. 가벼운 늘어남은 주변에 증가 |

**남은 일:** 큰 겨드랑이 홈과 앞 들기, 큰 몸통 비틀림은 이 작업으로 완료되지 않았습니다. 보정이 꺼지는 자세는 해결된 자세로 세지 않았습니다. [색 전달 실패 비교](avatar_modeling/v093_bake_projection/TEXTURE_COMPARE.png) · [옆 들기 전후](avatar_modeling/v119_garment_review/BOARD_side_BACK.png)

<a id="t05"></a>
### T05 · 팔 회전 경로와 키 보간 오류를 분리

**문제:** 앞으로 들기 명령에 옆 벌림이 섞였고, 정지 그림은 정상이어도 재생 중 팔이 뒤로 돌아가는 구간이 생겼습니다.

| 순서 | 수행·관찰 | 판정과 다음 연결 |
|---|---|---|
| [v121 조작 축](avatar_modeling/v121_arm_controls/START_HERE.md) | 벌어진 기본 팔 자세에 맞춰 조작용 4본 추가, 기존 102본의 기본 위치 유지 | 앞뒤 경로 개선. 접촉 문제는 별도이며 증가한 사례도 기록 |
| [v132 쇄골·표면·재생](avatar_modeling/v132_shoulder_review/START_HERE.md) | 쇄골 앞쪽 이동 분담과 제한 표면 보정 비교 | 넓은 이동 실패안 제외. 주요 보호 자세 유지 |
| 같은 v132의 연속 검사 | 동일 회전을 나타내는 quaternion 부호가 키 사이에서 뒤집힘 | 부호를 연속 정렬한 뒤 793개 정수·반프레임 표본 재검사 |

**남은 일:** 더 자연스러운 동작 경로가 모든 의상 표면을 해결하지는 않습니다. [v121 경로 비교](avatar_modeling/v121_arm_controls/BOARD_FRONT90_100.png)와 [v132 후속 비교](avatar_modeling/v132_shoulder_review/BOARD_front90_100.png)는 서로 다른 기준에서 출발한 그림입니다.

<a id="t06"></a>
### T06 · 어깨선의 색·음영과 실제 소매 두께

2026-09-12 · [v328 참고 모델 팔 모으기](avatar_modeling/v328_reference_elbow/v328_REFERENCE_REVIEW.md): Lapwing 볼레로·SiuSiu 블레이저와 비앙카의 실제 팔 방향을 맞춰 비교했습니다. 두 참고 모델에도 접힘·겹쳐 보임이 있지만 옷 구조와 입력 조건이 달라 일반화하지 않습니다. 모델 수정 없이 원래 재질8장과 검사 한계를 기록했습니다. 사용자 의견에 따라 v327 둥근 측면은 채택 보류하고 수정 전 측면을 보존합니다.

2026-09-12 · [v327 팔꿈치 각짐 수정](avatar_modeling/v327_elbow_fix/v327_ELBOW_FIX_REVIEW.md): 깊게 굽힐 때의 사각 돌출을 보정2키로 완화했습니다. 기존 형상·UV·본·웨이트·디테일을 유지하고135°부터 최대75% 적용합니다. 정적23·연속181표본과 중립 복귀를 확인했습니다. 새 소매–몸 교차0이지만 안쪽 자기 교차와 강한 모으기 자세는 남습니다. 사진·부피 변화와 [미채택 시도](avatar_modeling/v327_elbow_fix/v327_ATTEMPTS.md)를 함께 기록한 별도 후보입니다.

**문제:** 검은 선, 날카로운 음영, 실제 함몰이 겹쳤고 앞 들기에서는 소매가 납작해졌습니다.

| 순서 | 수행·관찰 | 판정과 다음 연결 |
|---|---|---|
| [v139 분리 진단](avatar_modeling/v139_seam_diagnosis/START_HERE.md) | 색·회색 표면·노멀 경계로 원인 구분 | 텍스처만으로 형상 문제를 고쳤다고 판단하지 않음 |
| [v146 어깨선 기준](avatar_modeling/v146_seam_topology/START_HERE.md) | 원래 선을 유지하며 보정 길이와 면 한 곳 분할 수정 | 정장 선을 지우는 텍스처안 제외 |
| [v152 두께](avatar_modeling/v152_volume_review/START_HERE.md) | 기존 코트 493개 정점에 양쪽 60·90·120도 보정 6개 | 앞90 두께 비율 66.6%→89.8%. 새 면적·접촉 경고가 남은 비교 후보 |
| [v154 보조 구조](avatar_modeling/v154_shoulder_support/START_HERE.md), [v155 웨이트 적합](avatar_modeling/v155_weight_fit/START_HERE.md) | 후속 보조 구조와 웨이트 적합 실험 | 모두 통과한 개선으로 자동 통합하지 않음 |

**남은 일:** 큰 어깨·겨드랑이 접힘은 미해결입니다. 두께는 같은 구역의 회전을 정렬한 앞뒤 범위로, 닫힌 부피가 아닙니다. [정지 확대](avatar_modeling/v152_volume_review/COMPARE_F90_DETAIL.jpg) · [선택 구간 GIF](avatar_modeling/v152_volume_review/FORWARD_PREVIEW.gif)

<a id="t07"></a>
### T07 · 카라를 팔에서 몸통 쪽으로 완만하게 분담

**문제:** 팔을 들 때 카라가 상완을 따라 늘어났습니다. 참고 아바타의 본 이름만 복사하기보다 실제 구동과 부위 역할을 확인했습니다.

**경로:** [v156 구조 비교](avatar_modeling/v156_three_avatar_comparison/START_HERE.md) → [v157 실제 5구성×38자세](avatar_modeling/v157_reference_pose/START_HERE.md) → [v159 카라 전이](avatar_modeling/v159_collar_support/START_HERE.md) → [v163 통합](avatar_modeling/v163_review_bundle/START_HERE.md).

기존 208개 코트 정점의 상완·쇄골 웨이트 일부를 가슴으로 옮기되 경계에서 갑자기 고정되지 않게 했습니다. 358자세에서 새 면적 경고 0, 앞90 가장자리 최대 늘어남 1.930→1.597배를 확인했습니다. **접촉은 생긴 곳과 사라진 곳이 함께 있으므로 잔여 과제**입니다. [위/아래 비교](avatar_modeling/v159_collar_support/COLLAR_COMPARE_OBLIQUE.jpg). 구매 모델 원본·이미지는 이 공유본에 포함하지 않습니다.

<a id="t08"></a>
### T08 · 헤어의 큰 흔들림 후보를 버리고 기본 모양·복귀 확인

**문제:** 유연하게 만들수록 작은 면이 찌그러졌고, 목 충돌체는 정지 상태에서도 머리카락을 밀어냈습니다.

**경로:** [v160 FLEX/SMOOTH와 충돌체 비교](avatar_modeling/v160_hair_dynamics/START_HERE.md) → [v163 통합](avatar_modeling/v163_review_bundle/START_HERE.md) → [v175 연속 구동](avatar_modeling/v175_humanoid_reference/START_HERE.md).

기존 6개 헤어 체인을 바탕으로 부모·끝점을 추가하고 웨이트를 부드럽게 나눴습니다. FLEX의 약 14mm 움직임은 작은 면의 43건 경고로 제외했습니다. 최종 SMOOTH는 OFF 대비 최대 약 3.33mm, 정지 후 거의 원위치 복귀를 확인했습니다. 목 충돌체 반지름 조절로 기본 상태의 약 2.94mm 밀림도 제거했습니다. 같은 입력으로 ON/OFF/충돌체 OFF 각각 720프레임을 검사했습니다.

**측정 수정도 기록:** 초기 Unity 표면 좌표에는 스케일을 중복 적용한 오류가 있어 수정 뒤의 BakeMesh 결과를 사용했습니다. 초기 숫자와 최종 결과를 섞지 않습니다. **미확인:** 실제 VRChat 및 임의의 월드 바람. [비교 이미지](avatar_modeling/v160_hair_dynamics/SMOOTH_FRONT.jpg)

<a id="t09"></a>
### T09 · 코트 객체를 나눈 뒤 원래 음영 복원

**경로:** [v161 분리·실패·복원](avatar_modeling/v161_separate_coat/START_HERE.md) → [v163 통합](avatar_modeling/v163_review_bundle/START_HERE.md).

기존 BodyHair·Coat 정점과 면을 객체별로 나눴지만, 초기에는 좌표가 같아도 음영이 달랐습니다. 원래 면 모서리의 대응을 따라 사용자 지정 노멀과 sharp/smooth 정보를 복원했습니다. 78자세의 최대 좌표 차이는 약 0.00006mm, 6쌍 렌더의 최대 색 차이는 1/255였습니다. **이 작업은 역할 분리를 위한 외형 보존 검사이며 새 의상이나 대체 메시 제작이 아닙니다.** [분리 전후 비교](avatar_modeling/v161_separate_coat/SEPARATION_FRONT.jpg)

<a id="t10"></a>
### T10 · 소매 끝을 손과 전완 사이에 배분

**문제:** 손을 많이 따라가면 소매가 접히고 전완에만 고정하면 손이 소매를 통과했습니다.

**경로:** [v162 전완 Twist 실패](avatar_modeling/v162_forearm_twist/START_HERE.md) → [v165 소매 역할 비교](avatar_modeling/v165_cuff_roles/START_HERE.md) → [v180 통합](avatar_modeling/v180_integrated_review/START_HERE.md).

손목 축에 맞춘 보조 본 2개가 손 로컬 회전의 65%를 따라가도록 하고 소매 끝·단추 웨이트를 조절했습니다. 실제 490자세에서 면적 경고 사건 167→15, 새 0을 확인했고 추가 144복합 자세에서도 새 경고 0이었습니다. **기존 피부 접촉은 남아 있습니다.** 최초의 잘못된 마스크 영역 대신 최종 진단 렌더로 비교했습니다. [기준](avatar_modeling/v165_cuff_roles/FINAL_BASE_Rplus_FRONT.png) · [분담 후](avatar_modeling/v165_cuff_roles/FINAL_CUFF_Rplus_FRONT.png)

<a id="t11"></a>
### T11 · Blender 보정을 Unity SDK에서 자동 재현

**문제:** Blender 제약·드라이버가 그대로 전달되지 않았고, 가져오기·보간·레이어 조합·루트 이동마다 다른 오류가 드러났습니다.

| 순서 | 오류와 처리 | 확인 범위 |
|---|---|---|
| [v167 이식](avatar_modeling/v167_unity_integration/START_HERE.md) | 작은 웨이트 복원, Contacts/Animator 보정 구성, 방향에 맞는 보간 방식 선택 | 계산 결과와 실제 구동을 비교 |
| [v170 실행](avatar_modeling/v170_runtime_validation/START_HERE.md) | 바람 층과 보정 층의 Write Defaults 불일치 수정 | 값을 강제로 넣은 시연과 자동 실행을 구분 |
| 같은 v170의 공간 검사 | 루트 이동·스케일에 따른 Contact 동작 불일치를 측정 구역 6개로 분리 | 32×3스케일×2회전=192조건, 루트 이동 검사 |
| [v173 실행 구조](avatar_modeling/v173_fast_correctives/START_HERE.md) | FX 층 4→3, 파라미터 136→100 | 기능을 유지하며 중간 계산 감소. 연속 확인은 [T12](#t12) |

**남은 일:** SDK가 목표 키 값을 재현한다는 검사와 그 키가 모든 자세의 모양을 개선한다는 검사는 다릅니다. 실제 클라이언트 동작은 아직 확인하지 않았습니다.

<a id="t12"></a>
### T12 · Humanoid 정의와 실제 본 연결·연속 재생을 일치

**문제:** 초기 대기 자세에서 팔이 뒤로 모였고, 손가락 이름이 정의 목록에 있어도 실제 연결되지 않았습니다.

**경로:** [v174 초기 연속 검사](avatar_modeling/v174_continuous_qa/START_HERE.md) → [v175 기준 자세·매핑 수정과 재검증](avatar_modeling/v175_humanoid_reference/START_HERE.md) → [v180 통합 그림](avatar_modeling/v180_integrated_review/START_HERE.md).

원래 메시와 본 좌표를 유지하면서 Humanoid Avatar의 T-pose 기준을 교정했습니다. 손가락은 Unity의 정확한 이름 형식을 적용하고 실제 API가 51개 본을 반환하는지 확인했습니다. 물리 ON/OFF로 1,200프레임을 재생했고 2프레임 지연을 맞춘 키 오차는 약 0.00000742였습니다. **초기의 잘못된 매핑 상태에서 나온 동작은 최종 검증으로 사용하지 않았고, 2프레임 지연은 남습니다.** [이전 대기 자세](avatar_modeling/v180_integrated_review/Old_Humanoid_Idle.png) · [교정 후](avatar_modeling/v180_integrated_review/Fixed_Humanoid_Idle.png)

<a id="t13"></a>
### T13 · 등급과 실행 비용을 함께 보고 기능 유지

**경로:** [v170 성능 비교](avatar_modeling/v170_runtime_validation/PERFORMANCE_COMPARISON.md) → [v173 실행 구조 간소화](avatar_modeling/v173_fast_correctives/START_HERE.md) → [v180 통합 판단](avatar_modeling/v180_integrated_review/START_HERE.md).

비앙카는 Contacts 94개 등으로 PC Very Poor 등급입니다. 같은 Unity Editor의 유효 600표본에서 PhysBone 작업 시간의 합은 비앙카 약 0.696ms, SiuSiu 약 0.445ms, Lapwing 약 1.210ms였습니다. 사용자 판단에 따라 물리와 보정을 유지했습니다. 이후 FX 층을 줄이는 개선은 기능 삭제와 구분했습니다.

**측정 한계:** 이 값은 병렬 CPU 작업 시간의 합이며 GPU 시간·전체 프레임 시간·실제 VRChat 클라이언트 성능이 아닙니다. 참고 모델보다 항상 빠르다는 순위로 쓰지 않습니다.

<a id="t14"></a>
### T14 · 확대 검사에서 실패한 웨이트안을 제외하고 면 10곳만 전달

**문제:** Unity에서 남은 손·셔츠·골반 압축을 줄이려 했지만 개발 자세에서 통과한 수정이 추가 자세에서 실패했습니다.

| 순서 | 수행·관찰 | 판정과 다음 연결 |
|---|---|---|
| [v181 부위 진단](avatar_modeling/v181_joint_localization/START_HERE.md) | 실제 Unity 233자세에서 몸의 경고를 부위별 분리 | 예측 표면과 실제 BakeMesh의 일치를 확인한 뒤 실험 |
| [v182 골반 웨이트](avatar_modeling/v182_pelvis_weights/START_HERE.md) | 골반 웨이트 후보가 개발 233자세에서 새 0, 추가 433자세에서 새 43건 | 미채택 |
| [v183 어깨 웨이트](avatar_modeling/v183_shoulder_local_weights/START_HERE.md) | 제한 어깨 웨이트 후보가 433자세에서 새 189·해소 187건 | 미채택 |
| [v184 기존 면 대각선](avatar_modeling/v184_body_diagonals/START_HERE.md) | 손 6·셔츠 2·바지 2곳만 변경, 정점·UV·키·웨이트 유지 | 검사 기록에서 새 면적·교차 0. 실제 SDK 533자세의 정점 위치가 기준과 같음 |
| [v185 면 연결](avatar_modeling/v185_web_edgeflow/START_HERE.md) | 후속 연결 흐름 실험 | 통과 후보 없이 미채택 |
| [v186 전달 검토](avatar_modeling/v186_review_bundle/START_HERE.md) | 면 10곳의 수정만 반영한 자산 13개 패키지 재가져오기 | 참조·본 매핑·보정 구동 재확인. 이전 v180 환경이 있던 검수 프로젝트 |

**남은 일:** 큰 어깨·겨드랑이 접힘, 깊은 골반 굽힘, 일부 손 자세와 실제 VRChat 검증. 233·433·533 기록은 반복과 선별용 표본을 포함하므로 독립 시험 1,199개로 합산하지 않습니다. [손 국소 수정 전](avatar_modeling/v184_body_diagonals/BEFORE_Combined_39_HAND.png) · [후](avatar_modeling/v184_body_diagonals/AFTER_Combined_39_HAND.png)

## 이 기록을 이어갈 때

후속 공유에서는 기존 판정을 지우지 않고 해당 추적 항목에 새 버전의 **출발 기준·실제 변경·검사 조건·반영 여부·남은 문제**를 연결합니다. 해결된 문제는 해결 근거를 적고, 다시 문제가 발견되면 발견한 자세와 후속 결정을 덧붙입니다. 새 문제만 새 추적 번호를 부여하며, 진행 중 실험은 검토가 끝난 공유 결과와 섞지 않습니다.

현재 문서의 추적 번호와 버전 링크는 수동으로 관리합니다. 원시 데이터 대신 공개 보고서와 선별 그림으로 판단 근거를 따라갈 수 있게 구성했습니다. 모든 세부 보고서를 찾을 때는 [전체 단계 색인](STAGES.md)을 사용하세요.

### T15 · Mac 재현과 관절 품질 완료를 구별

Windows 인계6후보 → Mac 지문/동작 재생 → [v257 Mac 재현 검증](avatar_modeling/v257_mac_reproduction/START_HERE.md)에서 같은 결과 확인. 환경 재현은 완료했으나 기존 겨드랑이·골반의 기하 결함과 손 접촉은 별도 미해결입니다. 후속 검수 중 실험은 이 보충에 포함하지 않았습니다.


## T04 후속 보충 · 원래 겹면과 동작 회귀

2026-09-12 선별 보충: [v273 원래 코트 겹면 분리](avatar_modeling/v273_neutral_layer_repair/START_HERE.md)와 [v275 동작 회귀 검증](avatar_modeling/v275_rest_repair_validation/START_HERE.md)을 추가했습니다. 정지 자세의 자기 교차는 해결했지만 동작 회귀 때문에 미채택입니다. v187 이후 전체 작업이나 아바타 완성본을 공유한 것이 아닙니다.

기존 면·본·웨이트를 보존한 정지 위치 수정도 동작에서는 회귀를 만들었습니다. 원래 자기 교차 해소와 자연스러운 관절 변형을 별도로 검증하며, 두 검사를 함께 통과하기 전까지 안전 기준에 합치지 않습니다.

## T04 후속 보충 · 부피와 노멀, 실제 스키닝을 구분

[v295 소매 부피와 실제 스키닝 검수](avatar_modeling/v295_pose_target_skinning/README.md): 교차 없는 목표의 두께 손실을 실제 평면 절단으로 발견하고 단면적·최소 폭을 함께 수정했습니다. 음영 과장은 동일 좌표의 노멀 비교로 따로 분리했습니다. 실제20자세 스키닝은 검증했으나 자동 보정 입력의 회귀와 몸 접촉이 남아 미채택입니다.


[v308 부피 계산 수정과 형상 재검수](avatar_modeling/v308_smooth_local_contact/README.md)에서는 기준 혼용, 단면 교차점 수에 의존한 폭, 열린 몸 표면의 거리와 최근접 면 전환을 분리했습니다. 앞30도 두 자세의 구간 외곽 부피는 중립의 약96.5–96.7%로 유지되지만 복합 자세의 덩어리 접힘이 사진에 남아 미채택입니다. 이번 결과는 정적 형상 검수이며 자동 구동이 아닙니다.

[v315 원형 주름과 두께 키 분리](avatar_modeling/v315_volume_key_audit/README.md), [v316 부피와 면 길이 검수](avatar_modeling/v316_volume_shape_balance/README.md)에서는 원래 조형 주름을 구분하고 중앙 부피와 면의 늘어남을 함께 검사했습니다. 복합 자세의 중앙 외곽 부피 약96.8%를 유지했지만 셔츠 접촉과 어깨 각짐은 남아 미채택입니다. [v318 안쪽 셔츠 분리 조건](avatar_modeling/v318_inner_shirt_clearance/README.md)도 선택한 제약에서 성립하지 않아 모델에 적용하지 않았습니다. 자동 구동이나 전체 관절 완료를 뜻하지 않습니다.


### T04 이번 작업 종합 확인 · v309–v318

[v318 이번 작업 확인 문서](v318_WORK_REVIEW.md)에서 전후 사진, 부피·늘어남 검사, 미채택 이유와 남은 작업을 한 번에 확인할 수 있습니다. 전체 아바타 완료본은 아닙니다.

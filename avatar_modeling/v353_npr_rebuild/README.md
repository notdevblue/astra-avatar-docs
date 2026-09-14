> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

[별도 회고: 재작업에서 배운 점과 다음 절차](v353_RETROSPECTIVE.md) — 이번 재작업은 폐기 상태이며 문서 작성만 진행했다.

> **최신 사용자 결정: 이번 텍스처 전면 재작업은 전부 미채택·중단.** [폐기 결정과 재작업 전 기준](v353_TEXTURE_REBUILD_REJECTED.md). 아래는 당시 작업 이력이며 후속 채택 근거가 아니다.

# v353 NPR 전면 재작업

[v353 눈꺼풀 복원·옷/신발 디테일 검토](v353_EYELID_RESTORE_AND_DETAIL_REVIEW.md) — face8 사선 결함 미채택, face9 실제 전후 검수본. 옷·신발10항목 약화와 복원 우선순위 기록.

[v353 사진 포함 중간 점검](v353_MIDPOINT_REVIEW.md) — 얼굴·헤어·디테일 피드백 단계. 어깨 V7과 Ribbon은 미채택이며 추가 실행은 사용자 피드백까지 중지.

[v353 표면 근접·BC7 압축 비교](v353_SURFACE_COMPRESSION_REVIEW.md) — 실제 GPU8맵·NPR6쌍·손 재촬영 확인, 원본 PNG/해상도 유지·텍스처 비용75% 감소. 현재 표면의 통합 시험용 선택이며 어깨·허리·물리 최종 합본은 검증 중.

[v353 실제 참고 모델 동작 비교](v353_REFERENCE_MOTION_REVIEW.md) — 네 동작의 영상·직접 검토와 헤어 물리 유지 범위. 다른 모델로 수용할 수 없는 자켓 접힘은 별도로 수정 중.

[v353 헤어29·어깨 자동 구동·자켓 후속 검수](v353_HAIR29_RUNTIME_COAT_REVIEW.md) — Unity64사진 직접·독립 검수, 전달 검증과 실제 물리 실패 원인. [다른 모델과 비교한 공통 한계 기록](v353_COMMON_MODEL_LIMITS.md). 전체 작업 진행 중.


[v353 어깨·헤어 안쪽·자켓 물리 후속 보고](v353_SHOULDER_HAIR_COAT_REVIEW.md) — 실제 전후 사진, 배경 혼입의 선형 베이크 수정, 어깨 노멀과 Cloth 원인 대조. 남은 채색 경계와 동작 회귀를 계속 수정 중.

[v353 머리 안쪽·물리 반복 및 역순 검수](v353_INNER_HAIR_PHYSICS_REVIEW.md) — hair20 Unity64장과 조명 제거 전후, 넥타이 중력·실행 순서 영향, Cloth 실패 원인과 어깨 후속 사본. 전체 작업 계속 진행 중.

[v353 정수리·머리 안쪽 및 넥타이 연속 검수](v353_CROWN_TIE_CONTINUOUS_REVIEW.md) — 신규 V18/V19 채색, 각 Unity 64장, 30fps 넥타이 전후 영상, 어깨 회귀와 Cloth 추가 원인 대조. 전체 계속 진행 중.

[v353 헤어 알베도·관절·물리 후속 검수](v353_ALBEDO_RIG_ROOT_REVIEW.md) — 새 일곱 시점 헤어와 Unity 48장 비교, 어깨 540표본 회귀, 넥타이 지지·Cloth 원인 대조. 전체 작업 진행 중.


[v353 넥타이 목띠 교정·헤어 투영·Cloth 후속 검수](v353_SURFACE_PHYSICS_FOLLOWUP.md) — 실제 전후 사진·GIF와 실패 원인. 전체 작업은 계속 진행 중.


[후속 헤어 전후·얼굴 환경광·카라·Cloth 진단](v353_CONTINUATION_REVIEW.md) — 전체 계속 진행, 측면 헤어·자켓 미완료.

[사진 포함 실행·실패 원인·중간 검수](v353_WORK_REVIEW.md) · [레퍼런스 아트 방향과 조사](v353_ART_DIRECTION.md)

현재는 사용자 요청에 따라 중간 점검을 위해 추가 실행을 멈췄다. 아래는 단계별 이력이다. 단추/전사/눈 구조/UV/재질/물리의 실제 시험을 기록하며 최종 통합이나 PC VRChat 클라이언트 합격으로 표시하지 않는다. 원본 보존, 기존 메시 수정, Blender Computer Use, 독립 검토, Unity 실제 구동 기준을 따른다.

[눈·얼굴 실제 63장 후속 검수](reviews/eye/v353_NATIVE_EYE_N_AND_FACE_LIGHT_REVIEW.md) · [단추 실제 32장 독립 검수](reviews/eye/v353_BUTTON_NORMAL_INDEPENDENT_REVIEW.md)

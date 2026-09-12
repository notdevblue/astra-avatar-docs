> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v330 · 측면을 유지하는 팔꿈치 수정 후보

[수정 전 / 수정 후 / SiuSiu 사진 비교와 시도 기록](v330_ELBOW_COMPARISON.md)을 먼저 확인하세요. 기존 정점36개에 깊은 굽힘 보정2키를 추가한 두 번째 사본입니다. 정면 외곽만 작게 줄였고 기준 측면은 유지했습니다. 비틀림 복합 자세의 자기 교차가 남아 최종 채택은 아닙니다.

전달: `bianca_v330_ELBOW_REVIEW.blend`, `Bianca_v330_ElbowReview.unitypackage`. 원형 v324와 보류 v327은 보존했습니다. 무릎·손가락·팔 길이·텍스처는 유지했습니다.

작업 저장소에는 `FINAL_CHECK.json`, `PROFILE_CHECK.json`, `FINAL_CONTACTS.json`, `BLENDER_BUILD.json`, `UNITY_BUILD.json`, `VISUAL_REVIEW.json`과 재현 코드를 보존합니다. 최종 키 생성은 `prepare_outer_trial.py` → `prepare_delivery.py`이며, `prepare_trial.py`는 미채택 첫 시도와 초기 실행 코드 생성용입니다. 최종 실행 코드에 덮어쓰지 않도록 구분합니다. `weight_trials.py`는 이전 검사 함수 재사용용이며 그 안의 모든 실험을 이번에 다시 수행한 것은 아닙니다.

문서 저장소에는 이 안내와 사진 보고서, 직접 검수한12그림만 공유합니다.

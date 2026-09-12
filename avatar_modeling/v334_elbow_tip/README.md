> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v334 · 오른쪽 팔꿈치 끝 미세 조정

[전후 확대 사진·검증·시도 기록](v334_TIP_REVIEW.md)

v333의 오른쪽 끝 수직 윤곽만 기존 13점에서 최대 1.396mm 조절한 별도 검수 후보입니다. 작은 모서리는 남아 있습니다.

전달 파일은 `bianca_v334_ELBOW_TIP_REVIEW.blend`, `Bianca_v334_ElbowReview.unitypackage`입니다. Unity 프리팹은 `Assets/BiancaElbowTipReview/Bianca_v334_ElbowReview.prefab`입니다. 오른쪽 키 이름 `V333_ElbowContour_R`는 기존 컨트롤러와 호환되도록 유지했습니다.

`prepare_trials.py`가 작은 두 강도의 입력을 만들고 `prepare_delivery.py`가 선택된 델타를 준비합니다. `ElbowTipSetup.cs`, `build_blender.py`가 기존 모델의 별도 사본을 만듭니다. 최종 모델은 기존 32키 중 오른쪽 1키만 바꿉니다. `INCREMENT.npz`는 v333 대비 추가량, `POSE_KEYS.npz`는 해당 두 팔꿈치 키의 최종값입니다.

실제 실행은 `MacElbowTipProbeViews.Begin`, `MacElbowTipSweepViews.Begin`입니다. v334 전용 실행 상태 식별자를 사용해야 합니다. 두 실행 완료 후 `collect_native.py`, `check_final_contacts.py`, `final_metrics.py`로 최종 결과를 계산합니다. `FINAL_CHECK.json`, `FINAL_CONTACTS.json`이 최종 검증이며, `OFFLINE_CONTACTS.json`은 예비 계산입니다. `trials/initial_CONTACTS.json`의 새 교차 1쌍은 미채택 초기안입니다.

최종 16사진은 `prepare_final_views.py`, `MacElbowTipViews.Begin`으로 렌더했습니다. `v334_trial_*`는 예비 시험 사진으로 최종 실제 구동 사진과 구분합니다. 원본 보존·전달 파일·검수 사진의 해시는 각 보존/전달/시각 검수 JSON에 기록했습니다. 작업 저장소에 모델·코드·원시 결과를 함께 남기고 문서 공유 저장소에는 이 문서와 본문의 최종 사진만 게시합니다.

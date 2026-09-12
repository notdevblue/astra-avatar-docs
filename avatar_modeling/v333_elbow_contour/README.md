> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v333 · 팔꿈치 하단 윤곽 검수 후보

[원본·직전·수정본 24사진 보고서](v333_CONTOUR_REVIEW.md) · [시험과 미채택 이유](v333_ATTEMPTS.md)

최종 후보는 중앙 패임을 채우는 fill_iteration3입니다. 아래 각진 V자 윤곽은 줄었고, 바깥 작은 모서리와 원래 주름은 남아 있습니다. 사용자 최종 채택 전입니다.

## 전달 파일

- `bianca_v333_ELBOW_CONTOUR_REVIEW.blend`: v332를 바탕으로 새 자세 키 2개와 측정점 4개 추가.
- `Bianca_v333_ElbowReview.unitypackage`: 프리팹 `Assets/BiancaElbowContourReview/Bianca_v333_ElbowReview.prefab`.
- `POSE_KEYS.npz`: 기존 v332 위에 더하는 새 L/R 두 키의 상대 델타.
- `DELIVERY.json`: 최종 파일 해시, 범위, 검사 근거.

## 재현 순서와 실험 구분

최종 입력은 `trials/fill_iteration3.npz`입니다. `prepare_hinge_delivery.py` → `ElbowContourSetup.cs`의 빌드 / `build_blender.py` 순서로 만들었습니다. 실제 구동 검사는 `MacElbowContourProbe.cs`, 추가 검사 두 파일 `MacElbowContourSweepBefore.cs`, `MacElbowContourSweepAfter.cs`를 사용합니다. Unity 진입점 이름은 `MacElbowContourProbeViews.Begin` 등 Views를 포함합니다. 최종 사진은 `prepare_final_views.py`와 `MacElbowContourFinalViews.cs`입니다.

`FINAL_CHECK.json`, `FINAL_CONTACTS.json`, `SWEEP_CONTACTS.json`, `BLENDER_NATIVE_DRIVER_CHECK.json`, `CONTOUR_METRICS.json`은 최종 결과입니다. `final/`, `sweep_before/`, `sweep_after/`는 실제 실행 기록입니다. `SOURCE_PRESERVATION.json`에 원본 해시를 비교했습니다. `VISUAL_REVIEW.json`은 보고서에 넣은 30장의 확인·해시 기록입니다.

`prepare_delivery.py`는 초기 4키 후보용 과거 실험이며 최종 재현용이 아닙니다. `trials/obsolete_delivery_inputs/`의 GL/GR·REST 델타도 미채택 실험입니다. `rounding_*`, `rigid_*` 모델과 검사 결과는 이전 시험이므로 최종 전달 파일 대신 사용하지 않습니다. 웨이트·기본 좌표 변경 시험은 최종 후보에 적용하지 않았습니다.

작업 저장소에는 모델·코드·원시 결과를 함께 보관하고, 문서 공유 저장소에는 이 문서들과 본문에서 직접 사용한 비교 사진만 선별 게시합니다.

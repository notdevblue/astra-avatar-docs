> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v324 기존 디테일 복원

사용자가 지적한 v323의 신발·옷·머리 디테일 감소를 기존 v322 및 레퍼런스와 비교해 수정한 별도 검수본입니다. 얼룩 제거 범위가 정상적인 장식·주름·머릿결까지 넓어진 것이 원인이었습니다. 기존 위치에 맞는 디테일을 되살리고 오염 부위만 국소 정리했습니다.

- **[전후 사진·원인·시도·미채택 이유](v324_DETAIL_REVIEW.md)**
- **[코트 안·옆다리·손·소매 등 12그룹 사진](v324_SURFACE_GALLERY.md)**
- Blender: bianca_v324_DETAIL_REVIEW.blend (공유본 미포함: `bianca_v324_DETAIL_REVIEW.blend`)
- Unity: Bianca_v324_DetailReview.unitypackage (공유본 미포함: `Bianca_v324_DetailReview.unitypackage`), 내부 prefab `Bianca_v324_DetailReview.prefab`
- 전달 파일 해시 (공유본 미포함: `DELIVERY.json`), 실제 패키지 확인 (공유본 미포함: `PACKAGE_CHECK.json`), 직접 본 사진 목록 (공유본 미포함: `VISUAL_REVIEW.json`)

메시 형상·부피·원래 UV0·노멀·본·웨이트·26개 키는 보존했습니다. 새 `V324_UV`는 원래 배치와 밀도를 복사하고 작은 겹침 3면만 평행 이동합니다. Unity에는 UV 경계용 동일 위치 복제점 10개가 추가됩니다. 4K 아틀라스는 v323과 같은 2장입니다. 기존 v322/v323과 과거 관절 후보는 그대로 보존하며 전체 관절·VRChat 최종 완성 판정은 아닙니다.

## 재현과 확인

`build_detail.py`는 v323 Blender 사본을 읽고 v322의 기존 정합 텍스처와 v323의 정리 텍스처를 부위별로 혼합한 뒤 Blender의 EMIT bake로 두 아틀라스를 만듭니다. 새로운 그림 생성이나 기하 편집을 하지 않습니다. 입력 분류/오염 면/3면 UV 이동 JSON은 이 폴더에 기록했습니다. `prepare_hair_mask.py`와 `repair_local_uv.py`는 기록된 진단 입력으로부터 마스크·UV 이동을 준비한 과정입니다. 최종 재현에는 저장된 JSON을 사용합니다.

1. Blender에서 `build_detail.py`, `verify_native.py` 순서로 실행합니다. 후자는 실제 저장본을 다시 엽니다.
2. NumPy/Matplotlib 환경에서 `check_uv.py`, `check_surface_color.py`, `compare_detail.py`를 실행합니다.
3. `map_unity_uv.py`로 기존 Unity 메시 인덱스에 대응시킵니다. `DetailRestoreTransferViews.cs`는 기존 v224 prefab의 원래 정점·노멀·웨이트·희소 키 데이터를 정확히 복사해 검수 prefab/package를 작성합니다. 최종 두 PNG를 `Assets/BiancaDetailRestoreReview/`에 배치한 뒤 실행합니다.
4. `render_isolated.py -- AFTER`는 실제 표면 12그룹의 6방향 BaseColor 시트를 만듭니다. `DetailRestoreRenderViews.cs`는 같은 정적 좌표·노멀·카메라로 v322/v323/v324를 촬영합니다. PreviewScene을 닫고 사용자 장면 path/dirty 보존을 검사합니다.

상세 검사: Blender 재로드 (공유본 미포함: `RELOAD_CHECK.json`), UV (공유본 미포함: `UV_CHECK.json`), Unity 대응 (공유본 미포함: `UNITY_UV_TRANSFER.json`), Unity 속성 보존 (공유본 미포함: `UNITY_DELIVERY_CHECK.json`), 명암 비교 (공유본 미포함: `DETAIL_COMPARISON.json`), 표면 색 표본 (공유본 미포함: `SURFACE_COLOR_CHECK.json`). 정적 사진은 새로운 자동 관절 구동 시험을 대신하지 않습니다.

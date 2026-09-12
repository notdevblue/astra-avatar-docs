> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v325 전체 관절·본 검토

텍스처 v324 이후 현재 모델을 읽기 전용으로 검토했습니다. 본/웨이트 기본 구조는 통과했지만, 여러 관절의 시각적 접힘·접촉은 남았습니다. 이번에 수정 모델을 새로 만들거나 과거 후보를 통합하지 않았습니다.

- **[결과·사진·남은 문제·다음 우선순위](v325_RIG_REVIEW.md)**
- **[전체129본 점검표](v325_BONE_TABLE.md)**
- **[41장 사진 갤러리](v325_GALLERY.md)**
- 현재 모델: v324 안내 (공유본 미포함: `README.md`)

## 검사 자료

Blender 구조 (공유본 미포함: `BLENDER_INVENTORY.json`), 본 대응 (공유본 미포함: `BONE_CHECK.json`), Unity 구성 (공유본 미포함: `UNITY_INVENTORY.json`), 실제 자세·각도·본 움직임 (공유본 미포함: `LIVE_CHECK.json`), 부위별 면적 선별 (공유본 미포함: `SURFACE_SUMMARY.json`), 선별47자세 교차 (공유본 미포함: `CONTACT_CHECK.json`), 닫힌 구간 단면 (공유본 미포함: `SECTION_CHECK.json`), 사진 계획 (공유본 미포함: `RENDER_PLAN.json`), 원본 해시 보존 (공유본 미포함: `SOURCE_PRESERVATION.json`), 사용자 장면 보존 (공유본 미포함: `SCENE_PRESERVATION.json`).

검수 스냅숏 (공유본 미포함: `REVIEW_SNAPSHOTS.npz`)과 대응/해시 (공유본 미포함: `SNAPSHOT_MANIFEST.json`)는 실제 위치·노멀을 보존합니다. 소스 정점/삼각형/UV/웨이트는 두 `*_SOURCE.json.gz`에 있습니다. 모든 `.venv` 프레임을 Git에 올리는 대신 사진·교차 검수에 사용한 표본을 보존했습니다. 전체 검사 원문과 실행 코드도 이 작업 폴더에 있습니다. 공유 문서 저장소에는 보고서와 직접 본 그림만 게시합니다.

## 재현 순서

1. Blender에서 `native_inventory.py` 실행: v324 원본을 읽으며 저장하지 않습니다.
2. Unity의 현재 v324 prefab 의존성을 준비하고 `MacFullRigAudit.cs`를 Assets에 넣은 뒤 `MacFullRigAuditViews.Begin`을 실행합니다. 열린 사용자 장면을 변경하지 않고 Play Mode 내 임시 대상만 생성합니다. 관절은 바람0/PhysBone OFF, 보조 움직임은 별도 단계에서 ON입니다.
3. 출력 완료 후 `prepare_surface_map.py`, `analyze_surface.py`, `check_sections.py`를 NumPy/SciPy 환경에서 실행합니다. `check_contacts.py`는 Blender의 BVH를 사용합니다. 머리카락 표면과 공면 접촉은 정확 교차 집계에서 제외합니다.
4. `summarize_bones.py`, `make_render_plan.py` 후 `MacRigRenderViews.Begin`으로 실제 스냅숏을 촬영합니다. 원래 재질·노멀을 유지합니다.
5. `preserve_evidence.py`, `write_report.py`로 원형 보존과 보고서 숫자를 맞춥니다. 사진은 별도로 직접 확인합니다.

`MacRigRecovery.cs`와 `recovery/`는 기본 바람 애니메이션 때문에 복귀 차이가 발생한 원인 조사 자료입니다. `trials/default_wind/`의 초기 결과를 최종 바람0 관절 수치와 섞지 않습니다. 원시 경고를 가시 결함 개수·관통 깊이로 해석하지 않습니다.

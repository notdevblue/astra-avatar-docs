> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v003 · 머리카락·피부·눈 실제 질감 후보

- **[네 후보 비교와 선택 안내](v003_SURFACE_REVIEW.md)**
- [후보별 큰 얼굴·눈 사진](v003_CANDIDATE_GALLERY.md)
- [다른 각도·조명·검증과 남은 문제](v003_VALIDATION.md)

B_SOFT / C_SILK / D_WARM / E_CLEAR의4개 프리팹을 실제 저장했다. 기존 v001의 B–E와 다른 후보이며 모두 최종 미채택 검수본이다. v324를 동일한 재질 비교 입력으로 사용했고 최신 관절 후보에 자동 통합하지 않았다.

## Unity 전달물

`Bianca_Shader_v003_SurfaceCandidates.unitypackage`에 후보4개와 필요한 비앙카 자산39개가 있다. Unity2022.3.22f1, lilToon2.3.4 및 현재 VRChat SDK가 있는 환경을 전제로 하며 셰이더/SDK 패키지 자체는 내장하지 않는다. 빈 프로젝트 재수입과 VRChat 실행 검증은 아직이다.

실제 프로젝트의 `Assets/BiancaShader/v003_SurfaceCandidates/` 아래에서 `Bianca_Shader_v003_B_SOFT.prefab`, `Bianca_Shader_v003_C_SILK.prefab`, `Bianca_Shader_v003_D_WARM.prefab`, `Bianca_Shader_v003_E_CLEAR.prefab`을 찾을 수 있다. `UnityAssets/Assets/`에는 같은 자산과 GUID meta 사본을 보존했다. 원래 사용자 장면에는 배치하지 않았다.

## 재현·보존 자료

`build_surface_masks.py`는 v324 UV 삼각형을 보간해 효과 마스크와 수식 MatCap을 생성한다. 실행: `uv run --no-project --with numpy --with pillow --with scipy python avatar_shader/v003_surface_candidates/build_surface_masks.py`. 기존 MainTex는 수정하지 않는다.

`Masks/`와 `MatCaps/`를 같은 이름의 새 Unity 후보 폴더에 복사한 뒤 `MacSurfaceCandidateViews.cs`를 임시 Editor 폴더에서 컴파일한다. `Build`, `Render`, `Verify`, `Export` 순서로 실행하거나 `FinalizeCandidates`를 호출한다. 원래 v324/SDK/lilToon 의존성이 필요하고 Editor가 재생 중이면 변경 전에 중단한다. Render는 임시 Preview Scene을 닫는다. 보관된 UnityAssets 복원 시 meta도 함께 복원한다.

`BUILD.json`, `MASK_BUILD.json`, `VERIFY.json`, `RENDER.json`, `SOURCE_PRESERVATION.json`, `CLOTH_ROI_CHECK.json`, `PACKAGE_CHECK.json`, `VISUAL_REVIEW.json`에 조건과 검증을 기록한다. 위치맵 NPZ·마스크·수식 MatCap·실행 소스는 작업 저장소 전용이며 `.venv` 밖에 보존한다. `Trials/`에는 초기 홍채 경계 사진과 눈이 가려져 검증용으로 부족했던 이전 조명 사진을 보존한다.

공유 문서 저장소에는 검수 문서와 직접 확인한 실제 비교 사진만 게시한다. 모델·MainTex·마스크·MatCap·코드·원시 JSON/NPZ·인계는 공유 대상이 아니다.

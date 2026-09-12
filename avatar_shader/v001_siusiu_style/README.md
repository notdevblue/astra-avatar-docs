> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v001 · SiuSiu 방식의 비앙카 셰이더 시험

**최신 사용자 정정:** 목표는 SiuSiu의 질감·재질 표현이다. 테두리/하이라이트 띠 중심의 해석을 정정하고 E의 자동 후속 기준 지정을 보류한다. [사용자 피드백과 조사 방향](v001_FEEDBACK.md)을 먼저 읽는다. 아래는 정정 전의 실제 시험 기록이다.

**[사진 포함 결과·구현 방향](v001_SHADER_REVIEW.md)**

사용자 요청대로 `avatar_shader`에 만든 첫 셰이더 시험이다. 기존 비앙카 v324의 재질 사본을 만들고 B_FLAT/C_SOFT/D_STRONG/E_BAND를 실제 Unity에서 비교했다. **전체 마무리·사용자 최종 채택이 아닌 검수 후보**다.

## 확인할 파일

- Unity 검수 후보: `Assets/BiancaShader/v001_SiuStyle/Bianca_Shader_v001_E_BAND.prefab`
- 기존 환경용 전달 패키지: `Bianca_Shader_v001_E_BandReview.unitypackage`
- A는 기존 v324, B는 툰 그림자 OFF, C/D는 테두리형 MatCap 강도 차이, E는 테두리 대신 좁은 하이라이트를 사용한 후속 후보.
- `UnityAssets/Assets/BiancaShader/v001_SiuStyle/`에 재질·프리팹·마스크·MatCap과 GUID meta 사본을 보존한다. 이 경로는 소스 보관용이며 Unity는 실제 프로젝트의 `Assets/BiancaShader/`를 사용한다.

패키지는 Unity2022.3.22f1, lilToon2.3.4, 현재 VRChat SDK 패키지가 있는 환경을 전제로 한다. 셰이더/SDK 패키지를 내장하지 않는다. 구매 SiuSiu의 모델·재질·텍스처는 전달 패키지에 넣지 않는다. E는 검수용이며 VRChat 업로드·클라이언트 테스트를 완료한 파일이 아니다.

## 재현 소스와 검증 자료

`build_masks.py`는 v324의 UV·부위 분류에서 영역 마스크를 만들고 수식으로 원본 MatCap을 생성한다. `uv run --no-project --with numpy --with pillow --with scipy python avatar_shader/v001_siusiu_style/build_masks.py`로 실행한다. 기본색 텍스처를 편집하는 스크립트가 아니다.

`Masks/`와 `MatCaps/`를 프로젝트의 `Assets/BiancaShader/v001_SiuStyle/` 아래 같은 이름의 폴더로 복사하고 Refresh한 뒤, `MacBiancaShaderViews.cs`를 임시 Editor 폴더에서 컴파일한다. 또는 보관된 `UnityAssets/Assets/`를 같은 프로젝트에 GUID meta와 함께 복원한다. 원래 v324/SDK/lilToon 의존성이 먼저 있어야 한다.

`MacBiancaShaderViews.cs`의 `Build`는 별도 Assets 폴더의 후보 재질·프리팹을 작성하고, `Render`는 Preview Scene에서 비교 렌더를 만든다. `Verify`는 저장된 프리팹을 다시 읽어 메시·MainTex·변환·슬롯을 확인한다. `Export`는 E의 명시적인 프로젝트 자산 의존성만 패키지에 담는다. 기존 기준 파일을 수정하지 않는다.

`BUILD.json`, `MASK_BUILD.json`, `VERIFY.json`, `SOURCE_PRESERVATION.json`, `PACKAGE_MANIFEST.json`, `VISUAL_REVIEW.json`에 실제 조건·검사를 기록한다. `Review/pre_import_optimization`은 마스크 축소/압축 전 E의 비교 사진이다. 새로운 마스크8장 중 Shadow2장과 Regions2장은 준비/진단용이며 최종 E가 사용하는 효과 마스크는4장이다.

문서 공유 저장소에는 README/보고서와 직접 확인한 비교 그림만 게시한다. 모델·제작 마스크·MatCap·재질·코드·원시 검증 자료는 작업 저장소에서 보존한다.

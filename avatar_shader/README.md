> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# 비앙카 셰이더 작업

**현재 비교:** [v004 · 고정 그림자·2D 셰이더와 SiuSiu](v004_texture_shadow_comparison/v004_WORK_REVIEW.md). v343 텍스처를 유지한 B 검토 후보와 코트 명암을 덜 그린 C 실험 사본. 기존 후보는 아래 역사 기록으로 보존합니다.

**현재 실제 후보:** [v003 · 머리카락·피부·눈 4후보](v003_surface_candidates/v003_SURFACE_REVIEW.md) · [큰 얼굴·눈 사진](v003_surface_candidates/v003_CANDIDATE_GALLERY.md). B부드러움/C윤기/D따뜻함/E차분함, 최종 채택 전 검수본이다.

**소재별 후속 조사:** [v002 · 비앙카 의상 소재에 맞춘 적용 방향](v002_material_mapping/v002_MATERIAL_REVIEW.md). 기존 사진과 마스크 구분을 다시 검토했으며 새 재질 편집은 아직 없다.

**최신 방향:** SiuSiu의 표면 질감과 재질 표현이 목표다. [사용자 정정](v001_siusiu_style/v001_FEEDBACK.md)에 따라 기존 E의 후속 기준 지정을 보류하고 부위별 질감 분석부터 진행한다.

사용자 요청에 따라 셰이더 조사·실험은 `avatar_modeling`과 분리해 **`avatar_shader`**에서 관리한다. 이 폴더의 `v001`은 셰이더 작업의 별도 버전 번호다.

- **[v001 · SiuSiu 방식 실제 재질 시험](v001_siusiu_style/v001_SHADER_REVIEW.md)** — 현재/B/C/D/E와 SiuSiu 비교, 실패한 광택 시험, 후속 방향.
- [v001 후보 파일·재현 안내](v001_siusiu_style/README.md)

이전 [v332 원본 재질 조사](../avatar_modeling/v332_shader_research/v332_SHADER_REVIEW.md)는 역사 기록으로 유지한다. 셰이더 후보의 생성은 관절 후보·텍스처·전체 아바타의 최종 채택을 의미하지 않는다.

# v001 · 출처와 검증 범위

조회일: 2026-09-15. 제품 제작자와 도구 개발자의 1차 자료를 사용했다. 가격·구매 추천은 조사 범위가 아니다. 원문을 번역해 길게 옮기지 않고 필요한 사실만 요약했다.

| 자료 | 보고서에서 사용한 내용 | 범위·한계 |
|---|---|---|
| [ADPX · SiuSiu](https://adpx.booth.pm/items/8426340) | 기본 몸 키 112개, Kisekae/착장 메뉴, 2026-08-28 어깨 및 몸 관통 수정 | 제작자 명시. 현재 로컬 버전과 최신 제품 버전의 전체 일치는 확인하지 않음 |
| [久 · Lapwing](https://kujishift.booth.pm/items/4993931) | 착장/기본 몸 구성, 소매 슬릿 관통 주의 | 제작자 명시. 기본 몸의 모든 면·자세 품질을 검사한 자료는 아님 |
| [Modular Avatar · Merge Armature](https://modular-avatar.nadena.dev/docs/reference/merge-armature) | 의상 본 병합·참조 갱신, 특정 대상 아바타에 맞춘 의상 연결 | 체형 자동 피팅이나 관통 해결을 보장하지 않음 |
| [Modular Avatar · 의상 제작자 안내](https://modular-avatar.nadena.dev/docs/distributing-prefabs/for-outfit-creators) | 대상 골격·몸 가림·키 동기화, 조립 후 검수 | 비앙카에 해당 설정을 적용한 증거는 아님 |
| [Modular Avatar · Blendshape Sync](https://modular-avatar.nadena.dev/docs/reference/blendshape-sync) | 기존 대응 키 값 동기화, Animator 런타임 지원/내장 viseme·eye look 제한 | 새로운 옷 키의 자동 생성 기능이 아님 |
| [Blender · Data Transfer](https://docs.blender.org/manual/en/4.3/modeling/modifiers/modify/data_transfer.html) | 정점 그룹 등 데이터 전사 | 검색에 노출된 공식 4.3 문서 본문 요약으로 확인. 직접 본문 열기는 도구 오류, 최신 버전 UI 위치는 검증하지 않음 |
| [VRChat · PhysBones](https://creators.vrchat.com/common-components/physbones/) | Transform/본 체인, 힘·중력·충돌 설정 | 전신 옷 삼각형의 자동 충돌 시스템으로 해석하지 않음 |
| [VRChat · Avatar Optimization Tips](https://creators.vrchat.com/avatars/avatar-optimizing-tips/) | 스킨 메시 수 감소, Cloth 사용 제한 | 권장 사항이며 비앙카 FPS 실측이 아님 |
| [Avatar Optimizer · Merge Skinned Mesh](https://vpm.anatawa12.com/avatar-optimizer/en/docs/reference/merge-skinned-mesh/) | 렌더러 병합, 재질·키·개별 표시 제약 | 구성에 따른 병합 후 외형/전환 검수 필요 |
| [Avatar Optimizer · Remove Mesh By Mask](https://vpm.anatawa12.com/avatar-optimizer/en/docs/reference/remove-mesh-by-mask/) | 옷으로 가려진 면의 마스크 제거 | 전환 시 다시 필요한 면까지 제거하면 안 됨 |

## 로컬 근거

- 최신 작업 상태: `AVATAR_HANDOFF.md`, `avatar_modeling/v354_rig_recovery/v354_WORK_PLAN.md`를 현재 파일에서 확인. v352 기준, v353 표면 폐기, 기존 메시 제약을 반영했다.
- 현재 기준: `unity_vrchat_avatar/Assets/BiancaV352Review/Bianca_v352_Final.prefab` 및 BodyHair/Coat 메시. 슬롯 참조는 v352와 v351 재질 메타데이터의 GUID로 확인했다.
- 보존 확인: `avatar_modeling/v352_eye_shadow_research/DELIVERY.json`의 원본 Blend 및 Final.prefab과 현재 SHA-256을 비교했다.
- 참고 구조: `avatar_modeling/v335_uv_texture_audit/AUDIT.json`과 SiuSiu/Lapwing 원본 해시 기록. 현재 프리팹·메시 원본만 비교했으며 전체 텍스처·패키지·실행 환경이 같다는 주장은 하지 않는다.
- 사진: 같은 v335 폴더의 세 모델 `*_TORSO_ORIGINAL.png`. 원본과 복사본 바이트 일치. 이번에 직접 다시 열어 착장 차이를 확인했다.
- 결과: [공개용 확인 결과 요약](v001_VERIFICATION.md). 재실행 스크립트 `audit_local.py`는 작업 저장소 R&D 폴더에 보존한다.

공유 문서에는 보고서·설명용 사진·구조/해시 요약만 게시한다. 모델, 텍스처 원본, 참고 모델 정점/UV/웨이트 배열은 이 R&D 공개 선별본에 포함하지 않는다. 본문에 제시한 다음 단계의 작업 순서와 비용 평가는 위 자료를 종합한 판단이며 실제 수행 결과와 구분한다.

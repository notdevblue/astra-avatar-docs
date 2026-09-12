> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v322 레퍼런스 텍스처 재작업·UV 경계 정리

2026-09-12. **코트·바지·셔츠를 재채색하고, 다른 부위의 색이 옷에 투영되는 문제와 UV 패딩 덮어쓰기를 수정한 검수 사본**입니다. 원래 모델의 메시·본·웨이트·실루엣은 보존했습니다. 아래 사진은 생성 그림 자체가 아니라 새 4K 텍스처를 실제 Unity 모델에 적용한 결과이며, 직접 확대해 확인했습니다.

[전후 사진·문제 원인·시도와 미채택 사유](v322_TEXTURE_REVIEW.md)

- Blender: bianca_v322_TEXTURE_REVIEW.blend (공유본 미포함: `bianca_v322_TEXTURE_REVIEW.blend`)
- Unity: Bianca_v322_TextureReview.unitypackage (공유본 미포함: `Bianca_v322_TextureReview.unitypackage`). 기존 Unity 2022.3.22f1 + VRChat SDK + lilToon 프로젝트에 가져오는 아바타 자산 패키지이며 SDK·셰이더 패키지 자체를 포함하지 않습니다. 가져온 뒤 `Assets/BiancaTextureReview/Bianca_v322_TextureReview.prefab`을 확인합니다. 기존 v224의 같은 메시 자산과 lilToon 설정을 쓰며 텍스처만 별도 재질로 연결했습니다.
- 텍스처: Bianca_reference_basecolor_v322.png (공유본 미포함: `Bianca_reference_basecolor_v322.png`)
- Blender 재로드 검사 (공유본 미포함: `RELOAD_CHECK.json`), Unity 저장 검사 (공유본 미포함: `UNITY_DELIVERY_CHECK.json`), UV·축소 검사 (공유본 미포함: `UV_TEXTURE_CHECK.json`)

UV는 검사 후 **좌표 재배치 없이 섬 경계 색과 32px 패딩을 정리**했습니다. UV 재배치가 필요하면 함께 진행하라는 사용자 지시를 유지합니다. 사진③ 및 첨부 측면의 허용 실루엣 결정을 바꾸거나, 관절 후보를 합친 작업은 아닙니다. 확대 시 옷깃 부근의 희미한 투영 윤곽과 셔츠의 부드러운 주름 표현은 남아 있어 전체 텍스처 최종 채택으로 기록하지 않습니다.

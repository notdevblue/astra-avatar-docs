> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v323 재현·검증 순서

실행 기준: Blender 5.2.1 LTS, Unity 2022.3.22f1, 기존 VRChat SDK와 lilToon 프로젝트. 스크립트의 작업 루트는 현재 Mac 경로이므로 다른 PC에서는 먼저 해당 상수를 바꿉니다. 초기 입력은 v322 검수 모델·아틀라스와 v005 부품 분류입니다.

1. `prepare_uv.py`: 원래 면 전수 분류, 부위별 임시 펼침 사본, 새 UV 초안과 생성용 펼침 사진. 최종 원본 메시에는 용접이나 컷을 적용하지 않습니다.
2. `generated_*.png`, `generated_*_refine.png`가 승인된 작업 입력입니다. 생성 요청은 `IMAGE_PROMPTS.json`, `IMAGE_PROMPTS_REFINE.json`에 보존했습니다. 다시 생성하면 동일 픽셀이 보장되지 않으므로 납품 재현에는 저장 입력을 사용합니다.
3. `UV_REPAIR_FACES.json` 452면과 `HAIR_BLEED_FACES.json` 1면은 이번 진단에서 확정한 입력입니다. `find_hair_bleed.py`는 수정 전 결과를 분석했던 진단 도구이며, 최종 파일에 다시 실행해 이 목록을 덮어쓰지 않습니다.
4. `build_texture_uv.py`: 새 UV 수정, 생성 채색·원본 머리/얼굴 입력을 재질로 합성하고 Blender EMIT 베이크. 최종 파일·2장 아틀라스·면별 UV 대응표를 저장합니다. `BUILD_SETTINGS.json`에 활성 수식을 기록합니다.
5. `verify_native.py`를 Blender에서 실행한 뒤 과학 Python 환경에서 `check_uv.py`, `check_surface_color.py` 실행. 원형 보존·UV 겹침/늘어짐·표면 색을 확인합니다.
6. Unity Editor에 `TextureUVTransferViews.cs`를 설치합니다. `Dump()`로 기존 v224 prefab의 UV/삼각형을 추출한 뒤 `map_unity_uv.py`로 원래 면과 대응합니다. 같은 원본에서 이미 추출한 `*_UNITY_SOURCE.json`도 보존했습니다.
7. 최종 PNG 2장을 `Assets/BiancaTextureUVReview/`에 복사한 뒤 `TextureUVTransferViews.Begin()`을 실행합니다. 기존 본·키·웨이트를 유지하며 별도 prefab과 package를 저장합니다. 기존 사용자 장면은 저장하지 않습니다.
8. Blender `render_isolated.py -- BEFORE`, `-- AFTER`, Unity `TextureUVRenderViews.Begin()`으로 전후를 촬영합니다. Unity 자세 입력은 v322 `source/0_*.bin`의 실제 좌표·노멀입니다. 모델링 결과와 채색 차이를 혼동하지 않도록 동일한 좌표를 사용합니다.
9. 사진을 직접 열어 보고 `VISUAL_REVIEW.json` 및 납품 SHA-256을 기록합니다. 실행 성공이나 수치 통과만으로 시각 품질 합격으로 처리하지 않습니다.

`trials/`의 스크립트는 실패 당시 기록이며 위 재현 경로 대신 실행하지 않습니다. 초기 `bianca_v323_UV_TRIAL.blend`에도 UV 문제는 남아 있고 최종 결과는 `bianca_v323_TEXTURE_UV_REVIEW.blend`입니다. 모델·텍스처·입력·원시 JSON·코드는 작업 저장소에만 보존합니다.

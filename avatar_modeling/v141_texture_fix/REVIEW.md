> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v141 — 어깨에 그려진 검은 띠 국소 보정

v140 사본의 어깨 베이스컬러만 수정했다. 기존 캐릭터를 새로 생성하거나 UV 배치를 바꾸지 않았다. **UV 경계 검사에서 보완점이 발견된 중간 후보이므로 최종 확인은 v143을 사용한다.**

- [앞 90도 비교](COMPARE_F90.jpg)
- [뒤 35도 비교](COMPARE_B35.jpg)
- [뒤 60도 비교](COMPARE_B60.jpg)
- 저장 결과 (공유본 미포함: `BUILD.json`), PNG 픽셀 검사 (공유본 미포함: `PNG_CHECK.json`)
- UV 경계·축소 검사 (공유본 미포함: `SEAM_MIP_CHECK.json`)

내장 imagegen의 이미지 편집 모드로 기존 atlas의 작은 코트 영역을 정리했다. 입력은 TEXTURE_EDIT_INPUT.png (공유본 미포함: `TEXTURE_EDIT_INPUT.png`), 출력은 GENERATED_CLEAN_PATCH.png (공유본 미포함: `GENERATED_CLEAN_PATCH.png`)다. 출력의 UV 섬 윤곽이 미세하게 달라 직접 덮어쓰지 않았다. 출력 중 깨끗한 원단 부분을 소스로 삼아 Blender의 기존 UV와 원래 3D 위치 마스크로 통합했다. 실제 출력 이미지를 사용했고 새 메시를 만들지 않았다.

양쪽 어깨 뒤쪽에서 과도하게 어두운 texel에만 최대 85% 혼합을 적용했다. 4096² PNG에서 실제 변경은 3,452픽셀, 최대 채널 변화 23/255다. 마스크 밖 변경 0, 코트 외 다른 부품이 덮는 UV 영역 변경 0을 확인했다. 모델 좌표, UV, 웨이트, 노멀은 v140과 같다.

렌더에서는 어깨 위 검은 곡선이 완화됐다. 다만 604개 UV 경계에 각각 네 표본을 둔 검사에서 일부 색 차이가 증가했다. 원본 대비 RGB 벡터 거리 증가 최대는 4096에서 0.04224, 1024 축소에서 0.03215였다. 따라서 이 파일을 최종으로 종료하지 않고, v143에서 UV 여백 처리 후 재검사한다. 평균값만 보고 전체 경계가 개선됐다고 판단하지 않는다.

원본 atlas는 오래된 filepath가 아닌 v138 안에 실제 packed된 이미지를 추출했다. 원본 SHA-256은 `83b70eb30188fa0cb863c1d0b2a4294ad5be45b4f4aa754f40dcdf070fd3582b`다.

## 실제 imagegen 프롬프트

```text
Use case: precise-object-edit. Edit target: supplied square crop of an existing game avatar UV base-color atlas. This is a technical texture edit, NOT a new character or a rearranged UV map. Preserve the EXACT placement, outlines, sizes and orientation of every disconnected UV island and preserve the solid black spaces. Only retouch the broad dark diagonal stripe/shadow running from upper-left toward lower-right across the large dark brown cloth island in the upper-right and center of this image. Replace that stripe with the immediately surrounding subdued gray-brown coat fabric color; blend naturally but retain the subtle low contrast fabric shading. Do not brighten or recolor the cloth overall. Keep every other island, all the pale skin region at bottom-right, and all other folds unchanged. No added detail, text, stitches, borders or objects. Keep square composition and pixel-aligned layout as closely as possible. The edited island should have no deep dark cut line.
```

원래 도구 출력은 `[로컬 경로 생략]`이며, 프로젝트 내 사본을 위에 연결했다.

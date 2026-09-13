> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v351 헤어 UV 완화 독립 기술 검토

## 판정

현재 HAIR_RELAXED 배열에서 **새 UV 뒤집힘 0, 정규화 면적 1e−11 초과의 겹침 0, 퇴화 0**을 독립 재계산했다. C34의 초기 겹침 4쌍은 실제로 0이며, 148면의 연결된 island를 유지했다. UV 좌표만으로 서로 다른 물리 정점을 합치던 문제는 이번 key에 물리 위치 ID를 넣으면서 해당 회귀가 해소됐다.

단, **73개 의미 영역은 실제 84개 UV island**이며, C11/C34의 각 한 삼각형은 완화 전 UV 면적의 10%까지 줄었다. 최종 packing·해상도·색/노멀 bake 검수까지 통과했다고 표현하지 않는다. 별도로 C61의 3면이 입 부근인데 hair로 분류돼 있어 의미 분류를 확인해야 한다.

## 배열·연결 검증

- 헤어 73영역, 원래 8,925 polygon, 영역 간 polygon 중복 할당 0.
- 전체 헤어 원래 loop 33,934개. BodyHair 배열 59,396×2 / Coat 7,285×2, 전후 크기 동일, 최종 UV 전부 유한.
- 헤어 이외 loop 변화 0, Coat loop 변화 0.
- 실제 물리 edge 양끝과 UV 양끝이 함께 같은지로 island를 계산했다. polygon 내부 tessellation 대각선도 원래 SOURCE 삼각형을 사용했다.
- 전체 island 수는 완화 전후 84개로 유지된다. 두 개 이상으로 나뉜 영역은 C0(140+4), C2(191+3), C17(109+1), C18(149+3), C19(238+8+2), C24(142+1), C28(142+1), C29(137+1), C30(155+2), C35(163+2)다. 나머지 63영역은 각 한 island다.

따라서 작은 부속이 있는 의미 영역을 pack할 때 이 부속까지 무조건 한 연결 가닥으로 부르지 않는다. 반대로 몇 면 부속을 자동으로 옛 UV에 복귀시키는 것도 필요하지 않다. 실제 검수 가능한 레이아웃과 이름으로 남기는 것이 적절하다.

## 겹침·면 뒤집힘

원래 SOURCE의 삼각형마다 전후 signed UV area를 계산했다. 완화 중 새 부호 반전은 0이며, 정규화 doubled area 절대값 1e−12 미만인 퇴화 삼각형도 0이다.

Shapely triangle intersection을 정규화 chart 좌표에서 독립 계산했다. 구현 기준 면적 1e−11 초과 겹침은 모든 영역 0이다. 더 엄격한 1e−14 기준으로는 C11의 face3044/14019에서 **1.3151e−14** 면적의 1쌍이 남는다. 수치 접촉 한계 수준이므로 시각적 겹침 결함이라고 단정하지 않지만, “정확한 실수 산술에서 겹침이 절대 0” 대신 사용 임계값을 명시한다.

원래 unwrap부터 음수 area 삼각형은 C61 2개, C64 1개, C65 1개 존재했고 그대로 유지됐다. 같은 UV island 안에서 부호가 섞였으나 독립 확인 결과 해당 원래 물리면들이 공유 edge를 같은 방향으로 도는 winding 불일치였다. 새 UV가 면을 접어 겹친 회귀가 아니다. 원래 3D winding을 이번 단계에서 바꾸지 않는다.

- C61: polygon524/529/5660, 입 주변 x−.002…−.006, y−.034…−.037, z≈.8599… .8604.
- C64: polygon1317/1322.
- C65: polygon1320/1326.

C61은 실제 헤어 가닥이라는 명칭과 맞는지 추가 확인이 필요하다. 나머지 위치 및 signed area·면법선은 `/tmp/v351_hair_winding.json`에 기록했다.

## 왜곡과 디테일 보존 후속

C11 초기 겹침2→0, C34 초기4→0은 확인됐다. 그러나 SLSQP의 area 하한이 원래의 10%이므로 다음 두 면이 정확히 이 하한에 도달했다.

- C11 polygon3044: area ratio 0.0999999985, 중심 world(0.04142,0.01799,0.87174).
- C34 polygon1606: area ratio 0.0999999995, 중심 world(−0.03703,0.06307,0.92725).

새 UV 뒤집힘이 없더라도 이 면들의 텍셀 밀도와 방향별 늘어짐이 충분한지는 별도다. 두 면과 이웃의 checker/원본 디테일/최종 mip을 확인하고, 손실이 나타나면 해당 가닥의 seam/여유 분배를 조정한다. 전체 이미지 샤프닝으로 왜곡을 숨기는 방식은 피한다. 수치적으로 10% 면적이라는 사실은 최종 texel 수가 10%라는 의미는 아니며 뒤의 pack 배율도 필요하다.

`relax_hair.py`는 physical rounded position ID+UV identity로 정점을 묶는다. 현재 수정 영향 영역의 signed area는 정상이나, constraint가 기존 sign을 곱하지 않고 positive area를 요구하므로 앞으로 음수 winding 영역을 움직이는 경우에는 원래 sign 보존 또는 해당 영역 분리를 명시해야 한다. 이번 결과의 신규 반전은 실제 검사에서 0이다.

## Blender geometry 보존

읽기 전용 background Blender로 v348 최종 사본과 `trials/bianca_v351_HAIR_UNPACKED.blend`를 열어 비교했다.

- BodyHair: 31,037점 / 15,723면 / 59,396loop / shape key27.
- Coat: 1,736점 / 1,936면 / 7,285loop / shape key33.
- Armature: 129본.
- 좌표, edge/loop/polygon 인덱스, 실제 triangulation, corner normal, 원래 vertex group 이름·가중치, shape key 좌표, 본 head/tail/matrix/parent, object world matrix가 모두 동일하다.
- 새 영구 메시/object 0. solver proxy는 저장 전에 제거됐다.
- UV는 Attribute 층 삭제와 V351_UV 추가만 달라졌고, 나머지 기존 UV층 hash는 동일하다. Blender의 UV층 한도 때문에 Attribute를 제거하는 코드가 있으며, “모든 기존 UV층을 사본 안에 그대로 유지했다”라고 쓰면 안 된다. 원래 v348에는 Attribute가 보존돼 있다.

이 Blender 파일은 **HAIR_UNWRAP 시점**이며 HAIR_RELAXED의 최종 packed/baked 파일 보존 검사는 아니다. relaxed 단계는 JSON UV 배열만 출력하고 geometry를 쓰지 않는 코드임을 확인했다. 최종 저장본은 pack/bake 후 다시 보존 검사가 필요하다. 이번 독립 검사는 drivers/constraints 전체 RNA를 비교하는 검사는 아니다.

상세: `/tmp/v351_hair_final_technical.json`, `/tmp/v351_hair_blend_preservation.json`, `/tmp/v351_hair_winding.json`. 재현 코드도 같은 `/tmp` 접두어 파일로 보존했다. 프로젝트 파일과 Unity는 변경하지 않았다.

## 후속 C61 의미 분류 확정

읽기 전용 식별색/가림해제 렌더를 직접 확인한 결과 C61은 입 내부 구조로 판정했다. 원래 component19 전체88면이 동일 구조이고 기존 REGION_face/head 그룹에 속한다. C59/60/61/62/67/69를 `face / mouth_inner_detail (protected)`로 재분류하는 것이 타당하다. 원래 어두운 내부색과 기하 winding은 보존한다. 상세 및 실제 진단 그림은 `/tmp/v351_lip_classification.md`를 따른다. 이 후속 결과를 헤어 최종 가닥 수와 분류 확정에 반영해야 한다.

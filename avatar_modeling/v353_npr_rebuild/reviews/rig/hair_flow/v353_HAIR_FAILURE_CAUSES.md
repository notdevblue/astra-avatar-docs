> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../../../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v353 헤어 좌표 실패 원인별 기록

현재 CONNECTED12/COT40/HARD_PATH(V6)는 실제 사진에서 실패·미채택이다. LSCM/ABF native 결과도 UV gate에 실패하여 **베이크하지 않았다**. `DEFINED_SLIM`은 실제 collapse0까지 개선됐으나31면 뒤집힘이 남아 gate 실패·미채택이며 베이크하지 않았다. 원래 UV의 유효성을 유지하고 지정root/tip 방향만 정렬한 `EXISTINGUV_DEFINED`는 collapse0/새flip0으로 실제 사진 비교 준비를 마쳤다.

| 원인 | 실제 근거 | 수정 방향/현재 상태 |
|---|---|---|
| 전역 U/V 연결이 가닥 방향을 보존하지 않음 | COT40 chart119 root→tip U .439 / V .216. 실제 사진에서 등고선·계단 발생 | 전역 scalar field를 최종 채색 좌표로 사용하지 않음 |
| 원화 밀도 증가 | 원화 읽는 폭이 .15에서 .95로 늘며 약6.3배 선 밀도 | .15 폭 유지, 반복 모드 금지 |
| 내부 경로 hard constraint가 면 좌표를 붕괴시킴 | V6 550삼각형/표면1.1712%가 |det|<1e-10. chart150 14면은 U 정확 상수, 가닥 면적9.98% | 공유점/중심선 수치만으로 채택 금지. native 면 펼치기 검증으로 변경 |
| 원본 raw 정점 연결이 UV 섬과 다름 | 78개 실제 UV 섬 중67개가 raw에서 분리. 가닥 합계1,696 raw 연결조각/17,659점 | 원본 메시를 고치지 않고 동일UV섬+같은UV+실제좌표동일 aliases만 exact proxy에서 연결.9,396점/15,928면/78섬, 비다양체0 |
| 기존 helper의 Blender API 호환 오류 | Edit→Object 뒤 cached UV RNA 무효, 후속 proxy에서 MeshUVLoop.select 없음 | 첫 원본 임시층 시험은 저장본 재로드로 복구. 이후 원본층 수정 방식 폐기. proxy-only, 모드복귀 RNA 재취득, Blender5 공유선택과 pin_ensure 사용 |
| 단순 LSCM/ABF가 면적/뒤집힘 보장을 하지 않음 | 실제 LSCM collapse1,028/표면.655688%, mixed118. ABF857/.719269%, mixed28. 원래 V351 해당면 UV collapse0 | invalid 결과를 bake하지 않음. SLIM No Flip과 원래 유효UV similarity 초기값 준비 |
| Pin을 포함한 면의 실제 뒤집힘 | DEFINED_SLIM의31면/103.89mm²가 같은 실제 UV 연결 성분 안에서 부호 반대.25면 tip pin/6면 root pin을 직접 포함. 비선언 UV cut0/연결성78 | No Flip 옵션만으로 합격 처리하지 않음. pin 없는 native 결과를 사후 방향 정렬하는 비교 준비 |
| 기존 UV 초기값 보존에 대한 설명 정정 | 공식 일반 unwrap 소스의 slim.skip_init=false, Live Unwrap에서만true. 기존 similarity 좌표를 제공해도 일반unwrap이 그대로 초기화한다고 말할 수 없음 | 현재 UV 기반 minimize_stretch와 새 unwrap을 구분. supplied_uv를 actual_initializer로 기록하지 않음 |
| root/tip 경계 휴리스틱이 실제 지정 끝과 다름 | chart116 이전 tip z.904 대 지정tip면 z.867, chart119 .918 대 .842, chart150 .960 대 .909 | 기존 ALL_PANELS root_face/tip_face 재사용. boundary만 끝으로 한정하지 않음. 지도상의 root/tip은 기존 생성기 메타데이터이며 사람의 해부학 판정으로 과장하지 않음 |
| 닫힌 끝이 UV 내부에 있음 | 78개 모두Euler1 disk/기존 pins 모두boundary였지만 실제 tip는 다수 내부캡에 위치. 즉 genus/내부pin 오류가 아님 | 59개 큰가닥의 지정 끝을 사용하고49개에 원래 경계→내부 끝 seam561edge를 proxy에서만 추가. cut 이후 face 연결성78 그대로, 파편 증가0 |

새 `DEFINED_ENDPOINT_SEAMS_CDE.json`은 59개 큰 섬의 지정 root/tip을 따른다. 이 중7개는 지정 tip면이 작은 분리 cap에 있으므로 큰 가닥의 가장 가까운 기존 정점에 대응한다. 나머지19개 작은조각은 큰 가닥인 것처럼 임의로 늘리지 않고 기존 boundary 기준을 사용한다. root/tip은 실제 3D 안내 사진으로 확인해야 하며 원화만으로 확인했다고 주장하지 않는다.

현재 `gui_native_defined_strand_unwrap_draft.py`는 원본 UV8층/모든 pin·선택·키·웨이트·좌표를 읽기만 한다. 임시 proxy에만 seam을 설정한다. seam으로 갈라지는 root UV fan을 모두 같은 좌표에 pin하면 잘라놓은 선이 다시 닫히므로 endpoint마다 한 corner만 pin한다. Minimum Stretch의 No Flip을 확인하고20회로 제한했으나31면 실패가 실제 확인됐다. optional pin_endpoints=False와 relax_existing=True는 후속 분리 시험용이며 아직 native 성공이나 시각 채택을 뜻하지 않는다. native 결과의 삼각형 방향/면적 gate를 통과한 뒤에도 실제 앞/사선/뒤/끝/안쪽 렌더가 필요하다.

Blender 공식 근거: [UV 선택과 pin의5.0 API 변경](https://developer.blender.org/docs/release_notes/5.0/python_api/), [Minimum Stretch의SLIM 도입](https://developer.blender.org/docs/release_notes/4.3/modeling/), [현재 UV unwrap 방법과No Flip](https://docs.blender.org/manual/nl/5.2/modeling/meshes/editing/uv.html). LSCM보다 SLIM이 이번 모델에서 반드시 아름답다는 근거는 없으며 실제 결과로 판정한다.

이 문서는 내부 원인/원시 좌표 검토용이다. 공개 보고서에는 허용된 실제 모델 렌더와 선별한 설명만 사용하고 원시 UV/아틀라스/구매모델 데이터는 포함하지 않는다.

## 안전 비교와 실행 품질 분리

`HAIR_EXISTINGUV_DEFINED_CDE.json`은15,928삼각형 전체에 기존 V351 UV의 회전/크기 정렬만 적용한다. runtime UV와 geometry는 그대로이고 새 UV/seam도0이다. 기존 UV와의 상대 방향 뒤집힘0/면 붕괴0을 검사했다. source pixel은 신규 art_sources만 사용한다.

`gui_bake_connected_hair_draft.py`에 samples 인자를 추가하고 기본16으로 변경했다. 좌표patch SHA,해상도,실제 sample 수를 결과 JSON에 기록한다. 베이크 선의 aliasing 개선을 UV 방향 해결과 혼동하지 않는다. sample16의 시각적 효과는 실제 render로 확인해야 한다.

공식 구현 참고: [일반 unwrap 및 Minimize Stretch 구현](https://github.com/blender/blender/blob/main/source/blender/editors/uvedit/uvedit_unwrap_ops.cc), [UV parameterizer의 SLIM 옵션 전달](https://github.com/blender/blender/blob/main/source/blender/geometry/intern/uv_parametrizer.cc). 확인한 것은 upstream main이며 설치된 Blender 바이너리와 같은 commit이라고 주장하지 않는다. 이후 native audit에 Blender version/build hash를 함께 기록한다.

## 무pin SLIM과 경계 고정 후속의 실제 구분

무pin SLIM은 native collapse0/mixed0, 각왜곡 중앙2.835°/95분위26.211°를 얻었다. 실제 UV 연결성도78섬 유지, 비선언 cut0, 선언561edge 분리, 원본8UV와 기하가 보존됐다. 그러나 부모가 실제 앞/뒤 사진을 확인한 결과 컬을 가로지르는 두꺼운 선과 잘못된 세로흐름이 남아 **미채택**했다. 유효한 conformal 전개와 원하는 일러스트 선 흐름은 같은 기준이 아니다.

그 후 실제 seam topology9,957 UV노드/78 disk에서 지정root→tip 양쪽 경계를 엄격한 ellipse에 배치하고 positive Laplace를 푸는 한정 비교를 준비했다. Uniform 가중치는 chart162의 내부11면을 과압축했고, 물리각/길이 기반 mean-value는81면으로 악화했다. 둘 다 gate 실패 결과를 보존하고 베이크하지 않았다. 최종 비교 후보는77개ellipse + 실패한chart162 한 섬의 기존 유효UV 방향정렬을 명시적으로 보존한 혼합이다. 게이트 임계값은 바꾸지 않았다.

`HAIR_DEFINED_TUTTE_ELLIPSE_CDE.json` 독립 감사:15,928면, 원본UV/currentCDEworld오차0, 연결성78, 비선언cut0, 공유edge좌표오차0, 최소|det|5.5378e-10,collapse0/mixed0. 원본 runtimeUV를 새로 편 것은 아니며 새원화 전사용ArtUV만 준비했다.

단조성은 개선됐으나 왜곡 위험이 크다. 선언 root→tip shortest-edge 가이드1,807표본의V역행은기존106/SLIM459/Tutte6이다. 반면 정규화UV에서 가이드각 중앙값30.59°/48.16°/64.94°이고, Tutte의 실제 삼각형각 차이 중앙45.15°/95분위115.13°다. 따라서 **Tutte는 시각 비교용이며 채택/품질 합격이 아니다**. 실제 컬·앞뒤 끝·안쪽·심을 보고 결정한다.

이 방법은 [Floater의 Mean Value Coordinates](https://www.sciencedirect.com/science/article/pii/S0167839603000025)와 [libigl의 harmonic parameterization](https://libigl.github.io/tutorial/#harmonic-parametrization)에 제시된 양수 가중/경계 기반 접근을 참고했다. 무뒤집힘 이론을 실제 유한 정밀도에서의 충분한 텍셀 밀도나 아름다운 가닥 선으로 확대하지 않는다.

## 눈 면 정리 이후의 베이크 대응

현재 눈의 기존중복100면 삭제는 원래 면/loop번호를 이동시킨다. helper는 `V353SourcePolygon` FACE INT의 유일 ancestry로 원래polygon을 찾고, 해당현재polygon 안에서 원래UV(<2e-7)와 승인CDE세계위치(<3µm)가 동시에 일치하는 유일loop만 사용한다. 기존/현재loop 일대일 대응도 전체 검사한다. ancestry없으면 원래index정확일치만 허용한다.

`validate_only=True`는 자산 생성0인 사전검증이다. 오프라인 fake API 검사는 실제15,928삼각형 전체를 재번호해33,602원래loop/8,837polygon을 찾아냈고, 중복ancestry·잘못된UV·잘못된위치·중복corner를 각각 차단했다. 이것은 Blender실행 검증과 다르며 실제 현재파일 preflight 결과는 부모가 기록한다. 눈수정본을 원래파일로 되돌려 베이크할 필요가 없다.

Tutte 실제35°/180°사진을 독립 검토했다. 앞 큰 컬과 정수리의 닫힌 등고선 같은 링, 뒤쪽 가닥의 각진 물결/삼각 경계가 반복된다. V역행 감소와 무뒤집힘에도 실제머리선 흐름은 개선되지 않았다. **Tutte77+기존1도 시각적 미채택 의견**이며, 기존UV_DEFINED보다 좋은 후보로 올리지 않는다. 같은 convex ellipse 형태를 수치만 조절해 반복하는 근거가 되지 않는다.

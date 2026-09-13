> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v348 독립 기술 검토 기록

검토자는 프로젝트 자산·코드를 수정하지 않고 기존 파일과 실행 결과를 읽었다. 진단 스크립트·결과는 `/tmp`에 작성했다. 아래 **확인 완료**, **추정**, **실행 대기**를 최종 보고서에서 구분해야 한다. 최신 판정은 아래 TECH-009의 TailClearance0.4 + tie x0/z8 실제 재실행 결과다. TECH-006~008은 원인 발견·수정 전·예측 기록이며 최종 결과와 구분한다.

## TECH-001 — 텍스처 전사의 연속 좌표 가려짐 판정

기존 3×3 raster ID 후보 조회는 픽셀 중심을 덮지 않는 얇은 앞면을 누락했다. 실제 바지·신발의 앞면 누락 반례를 확보했다. 모든 투영 삼각형의 AABB를 8px 타일에 등록하고 query의 연속 화면 좌표에서 내부 여부·깊이를 계산하는 방식으로 후보 누락을 해결했다.

첫 native 구현은 극도로 퇴화한 헤어 삼각형에서 부정확한 역행렬/평면 계수 때문에 정점 깊이 범위를 2m 이상 벗어나는 잘못된 앞면 깊이를 만들었다. centered 2×2 역행렬, determinant/condition 수치 조건, 음수 barycentric clamp 및 정규화 후 정점 z 직접 보간으로 이를 수정했다.

**독립 검증 완료:** pantsL_front/sideL, shoesL_front, hair_front의 5,975개 ray를 독립 외적 barycentric 계산으로 전수 앞면과 비교했다. 앞면 기준 −2µm/동일/+2µm의 총 17,925개 판정이 native와 모두 일치했고 nohit는 0이었다. 기존 앞면 누락 두 반례와 헤어 global 2249 퇴화 반례를 포함했다. 잘못된 −0.8115m 깊이는 정상 −2.98218m로 바뀌었다. hair_front의 수치 불안정 면 262/16,084개는 등록에서 제외됐다.

**판정 범위:** 이는 선택 ray의 가려짐 산술 검증이며 모든 아틀라스 픽셀의 전사·색 경계·시각 품질 합격이 아니다. 생성 소스 자체의 번짐, source RGB 필터링, 투영 view 선택, 미전사 면은 별도 직접 검수가 필요하다.

근거: `/tmp/v346_stable_native_review.py`, `/tmp/v346_stable_native_review.json`.

## TECH-002 — v346 최종 원형 보존

`/tmp/v346_final_preservation.log`의 최종 `TECH_RESULT`는 v343 대비 모든 비교 대상의 `all_equal=true`다. 메시 좌표/면/edge/평가 삼각형, 모든 UV와 활성·render UV, vertex/corner normal, shape keys, vertex groups/weights, object transform/parent/modifiers/constraints/animation 및 armature bones/pose 비교가 포함된다.

Head 재질은 V343_UV, Paint는 V341_UV로 연결된다. Paint가 사용하는 모든 loop에서 V343_UV와 V341_UV의 차이가 0이므로 Paint 노멀 bake나 전사에서 V343_UV를 사용한 것은 현재 사용 면에 대해 좌표 불일치를 만들지 않는다.

이 보존 결과는 **v346 텍스처 후보**에 대한 것이다. v348 Final의 의도적 TailExtension 웨이트 변경과 셰이더/Animator 통합 결과를 자동으로 검증한 것은 아니다.

## TECH-003 — NprFlow/Plus 실제 SDK 물리 검토

v347 실제 60Hz SDK OnFrameCompleteLate 기록을 독립 계산했다. NprFlow/Plus 각 4,800프레임은 프레임 누락·비유한 수치 없이 기록되었다. 동일 원점 순차 실행, 90프레임 워밍업, 바람 10초와 자연 복귀 10초를 사용했다. 본 위치 단위는 head/chest 회전만 제거한 실제 월드 상대 좌표(m)다.

- 바람 tip 최대 변위: TailExtension 헤어 7.83mm/넥타이 6.30mm → NprFlow 헤어 16.84mm/넥타이 16.58mm. 이 값은 각 구간 첫 기록 위치 대비 최대값이며 미관 점수가 아니다.
- Flow 실제 PhysBone 최대 local 본각: 헤어 5.289°(제한25°), 넥타이 4.923°(제한12°). 최초6.324°는 hair_wind_02 애니메이션 anchor까지 포함한 값이므로 PhysBone 제한 비교에서 제외하도록 정정했다. 제한 0.1° 이내 근접 및 초과 프레임 0.
- 바람 종료 후 최종 위치 ±0.1mm 이내 안정: 헤어 0.417초, 넥타이 0.533초. 마지막 1초 잔류 흔들림0, 마지막 tip은 입력 전 마지막 warmup 위치와 정확히 같다.
- Flow와 Plus는 HeadNod~Wind 6개 구간의 모든 기록 본좌표가 정확히 같다. 따라서 이 구간의 실제 표면 차이는 웨이트 변경 비교에 적합하다.
- 첫 HeadYaw는 Flow↔Plus 최대1.243mm, Medium↔Flow 최대1.901mm의 실행 간 차이가 있다. 원인 미분리 상태이며 첫 구간의 차이를 순수 웨이트 효과라고 해석하지 않는다.

실제 Unity 적용 웨이트는 계획 대비 최대1.69e-7, 비대상 정점은 float 재정규화 최대1.79e-7 차이다. 본 이름/순서 동일, 최대 영향본4개, Plus 헤어 비중 최대0.80000016으로 계획 상한과 부동소수점 오차 내 일치한다. 이 시점의 선택은 Plus가 아닌 TailExtension 기반 NprFlowSafe였다. 최종은 TECH-009의 TailClearance 후속을 따른다.

### 콜라이더 검사 제한과 남은 여유

프리팹 콜라이더를 실제 크기/위치/축으로 재구성하고 기록된 동적 끝점에 헤어 구체 반경4mm, 넥타이2mm를 포함해 최소 간격을 구했다. 이 검사는 SDK의 실제 접촉 로그나 본 전체 선분/헤어 메시 표면 교차 전수가 아니다. Neck은 프리팹의 neck-to-chest 상대 관계를 기준으로 계산했다.

Flow/Plus에서 Head 콜라이더와 hair_01_02의 최소 여유는 −0.0639mm(바람0.483초)로 기존 +2.34mm보다 작다. Neck 최소 −0.1566mm는 Medium과 같고 기존에도 작은 겹침이 있다. 본각 제한 미포화를 ‘모든 충돌 없음’으로 확대할 수 없다. v348 Safe는 해당 chain01 바람 gain을2.25→1.9로 낮추었으며 **최종 실제 실행에서 Head 간격 개선을 재확인해야 한다**.

근거: `/tmp/v347_flow_independent.json`, `/tmp/v347_flow_extra.json`, `/tmp/v347_flow_colliders.json`.

## TECH-004 — v348 최종 감사 코드

Secondary 코드는 같은 원점 Original/Final을 순차 실행하며 입력은 LateUpdate(-20000), 캡처는 실제 SDK OnFrameCompleteLate 이후 수행한다. frameCount 중복 방지와 20초 callback timeout이 있다. 영상은 Translation/Wind의 Head/Tie만30fps, 나머지 시점·구간은6fps이며 본은60Hz다. 모든 영상이30fps라는 표기는 부정확하다.

RigAudit의 관절 PhysBone 비활성 감사와 Secondary의 실제 PhysBone 활성 감사를 분리했다. 관절 감사는 독립 humanoid 채널·복합 자세·121개 연속 입력 표본을 포함한다. 연속 입력은 yield null 후 callback snapshot을 기다려 표본 사이 최소2프레임이므로, 실제 unity_frame/time 없이 이를60Hz 121프레임이라 표기하지 않는다.

제작자에게 다음 보완을 전달했다.

1. RigAudit의 `.venv/v348_final_npr` SOURCE/bin/NORMAL은 사진 재현에 필요한 증거다. 버전 폴더의 비공개 Git 추적 경로에 보존·해시 검증해야 한다.
2. RigAudit의 Instantiate한 root도 runner 전용 scene으로 이동해 사용자 PlayMode 장면과 분리한다.
3. LIVE_CHECK/DONE의 후속 물리 근거 표기를 v347이 아닌 실제 v348/physics로 갱신한다.
4. Continuous 표본에 unity_frame/time을 기록하거나 정확한 표본 단위로 설명한다.

## TECH-005 — BakeMesh 좌표계 검증 완료

RigAudit 및 Secondary.Bake는 `BakeMesh(mesh,true)` 후 TransformPoint를 사용한다. FinalReview의 두 SkinnedMeshRenderer localScale는100이다. Unity API 문서의 useScale 설명상 배율 중복 가능성을 점검할 필요가 있으나 **아직 오류로 확정하지 않는다**.

반대로 기존 v344도 같은 코드로 BodyHair 중립 높이0.99935m, Coat 높이0.31006m의 정상 범위를 저장했다. 따라서 실행 검증 없이 false로 변경하지 않는다. 임시 PreviewScene에서 true/false 원시 bounds, 각각 TransformPoint, true+회전·이동만, source 월드 bounds, renderer.bounds, 실제 head-foot 길이를 대조하는 `/tmp/MacV348BakeScaleProbe.cs`를 제공했다. 실제 `/tmp/v348_bake_scale_probe.json`을 수신해 판정했다. 이 import에서는 true raw 높이0.009995m → TransformPoint0.9995115m가 source/실제 렌더 크기에 맞는다. false raw는0.9995115m이나 TransformPoint를 더하면99.95114m로 잘못 확대된다. 따라서 **현재 BakeMesh(true)+TransformPoint가 올바르며, 초기 이중 배율 의심은 미성립으로 정정한다**. 실제 v348 SDK 중립 mesh_samples도 높이0.999349m로 v344와 일치했다.

[Unity BakeMesh 공식 API](https://docs.unity3d.com/2022.3/Documentation/ScriptReference/SkinnedMeshRenderer.BakeMesh.html)
과거 실제 범위: `/tmp/v348_prior_bake_bounds.json`.

## 최종 실행 후 추가할 판정

- v348 Original/Final 실제 실행 오류/연속성/원점/복귀/각도/Head·Neck 콜라이더 간격.
- Safe chain01 바람 감소가 Head 접촉 여유를 개선했는지.
- 실제 BakeMesh 좌표계 probe 및 저장된 mesh 샘플의 단위 확인.
- 최종 통합 관절/표정/shape control의 실제 구동 및 기존 승인된 무릎·손가락 상태 유지.
- 최종 영상·사진 직접 시각 판정과 원본/의존 파일 보존. 기술적 안정성을 전체 시각 합격이나 VRChat 클라이언트 검증으로 확대하지 않는다.

## TECH-006 — v348 최종 물리 실행 수신 후 추가 판정

RUN error=null, Original/Final 각각4,800프레임 완료. 두 기록 모두 고정dt 0.0166666675초, unity_frame 간격1, 비유한 수치0이다. 원점/이동 입력차0이고 본각 제한 근접·초과0이다. 최종 Head 여유는 −0.02520mm로 기존 Flow −0.06390mm보다 개선됐으나 양수 확보는 아니다. 최악은 frame3731(바람2.183초)의 hair_01_02다. Neck 최소 −0.15656mm는 Medium과 동일하고 기존 Original도 −0.14282mm의 작은 겹침이 있다. 무충돌·Head 겹침 제거로 표현하면 안 된다.

Original과 Final의 마지막 헤어/넥타이 tip은 입력 전 마지막 warmup 위치와 정확히 같다. 최종 바람 최대 tip은 헤어16.84mm/넥타이16.58mm이며 제한 포화·발산·잔류 흔들림이 없다. 이 수치는 실제 메시 표면 교차·시각 합격과 별개다.

근거: `/tmp/v348_physics_independent.json`, `/tmp/v348_physics_colliders.json`, `/tmp/v348_physics_return.json`.

## TECH-007 — 실제 표면 교차 추가 검사: 수정 필요 발견

Translation20/Turn20/Wind40의 동일 시각2Hz 표본80개씩, 총160개 실제 SDK BakeMesh를 비교했다. 헤어↔나머지 BodyHair/Coat, tie 본 영향 삼각형↔나머지 BodyHair/Coat를 BVH 후 비공면 정확 교차선>0.05mm로 검사했다. 원래 같은 위치를 공유하는 연결 정점은 제외했다. 공면·헤어 자기 교차·표본 사이 연속 충돌은 검사 범위 밖이다.

Original에는 모든 표본에서 기존 교차1,151쌍(헤어-얼굴729/코트235/셔츠187)이 있다. Final은1,115~1,219쌍이다. 새 pair 누적은 Translation135, Turn78, Wind925지만 접촉 경계가 인접 삼각형으로 이동해 생긴 새 pair가 포함되므로 결함 수나 관통 깊이로 해석하지 않는다.

대표 공간 분리 결과:

- 헤어15866↔face1897, Wind2.5초: 교차선6.338mm. 머리 옆뒤/귀 부근 x−.028~−.033,y.873~.876,z−.014~−.017m. 같은 face1897은 Original에서도 hair15864와 교차하며 두 선의 최단거리0.422mm다. 기존 접촉 경계 이동 성격이 강하다.
- 헤어16182↔coat422, Wind4초: 교차선5.991mm. 새 target 삼각형이지만 가장 가까운 기존 헤어-코트 선에서0.619mm로 목 뒤 코트 접촉 경계 이동에 가깝다.
- **헤어15702↔shirt27809, Wind2.5초:** 원래 동일쌍 최소간격2.998mm→새 교차선3.418mm. 가장 가까운 기존 헤어-셔츠 선에서도7.828mm 떨어진 뒤목 카라 안쪽 새 영역이다. 단순 pair 교체로 묶을 수 없다. source component10/local2의 새 chain00 웨이트 연장 영역이다. 정점27739/32602/27741은 원래 head1.0에서 헤어 비중.511/.478/.517로 변경됐다.
- **tie26169↔shirt26172, Wind3초:** 원래 동일쌍 최소간격0.475mm→새 교차선3.154mm. Original 전체 tie-셔츠 교차0, Final40 pair-instance 발생. x−.005~−.006,y.741~.744,z+.064~+.066m의 넥타이 윗부분/매듭 아래 접경이다. tie_01 약99~100%인 시작부라 anchor 회전이 표면을 셔츠로 움직이며, 끝점 PhysBone 콜라이더 여유 검사로는 발견할 수 없었다.

두 명확한 새 표면 회귀는 제작자에게 전달했고 국소 수정 진행 중이다. Final/Original Wind3초 Tie와 Final TieSide 실제 사진을 직접 확인했으나 일반 크기 이미지에서 교차선 자체가 분명히 식별되지는 않는다. 기하 회귀 확인과 바깥으로 보이는 결함 판정을 구분한다.

근거: `/tmp/v348_surface_contact_review.json`, `/tmp/v348_surface_locations.json`, `/tmp/v348_contact_spatial.json`. 후속 수정 전 결과이므로 최종 무회귀 판정으로 사용하지 않는다.

## TECH-008 — 카라 회피 웨이트 예측

새 component10의 헤어 비중을 기존 후보의0.4배로 줄이고 head에 돌려주는 TailClearance를 고정된 이전 본 궤적에서 예측했다. 이 component는 원래 head100%이고 검사3구간에서 head/root 입력이 동일하므로, 해당 위치의 Original→Final 선형 보간은 LBS 웨이트 축소에 대응한다. 다른 면은 이전 Final 위치를 유지했다.

80개2Hz 표본의 α0.4 예측에서 component10↔셔츠 새 교차0, 문제 shirt27809 새 교차0이다. 인접 기존 접촉 경계의 헤어↔코트/얼굴 새 pair는 남지만, 분리된 뒤목 카라 새 영역을 줄이는 목적에는 적합하다. 실제 SDK 재실행 결과로 재확인해야 하며 이 예측을 최종 무교차 검사로 대체하지 않는다.

근거: `/tmp/v348_tail_alpha_review.py`, `/tmp/v348_tail_alpha_review.json`.

## TECH-009 — 최신 실제 재실행: 두 표면 회귀 수정 확인

TailClearance(component10 새 헤어 영향0.4배/cap.24), Safe tie anchor x0/z8을 적용한 최신 실제 Final JSON 및 메시80시각을 다시 읽어 독립 검증했다. Original은 같은 입력의 첫 실행을 보존해 비교했다.

**두 회귀 수정 확인:** Translation20/Turn20/Wind40의 실제2Hz 표본 모두 새 넥타이-셔츠 교차0, 새 헤어-셔츠 교차0이다. 뒤목 카라27809 및 넥타이 시작부26169/셔츠26172의 발견된 회귀가 사라졌다. 남은 새 pair는 기존 인접 접촉 경계의 헤어-코트/얼굴로 Translation81/Turn41/Wind502다. 해당 수치를 전체 교차0 또는 모든 새로운 노출 결함0으로 확대하지 않는다.

**최신 운동 수치:** 헤어/넥타이 바람 tip 최대16.827/15.865mm, 종료 후 최종±0.1mm 안정0.417/0.517초, 마지막1초 잔류 흔들림0, 입력 전 마지막 warmup 대비 최종복귀0이다. 4,800프레임 연속·고정dt·유한성 통과, Original과 root입력 차0이다. 실제 PhysBone 본만 포함한 최대 local 회전은 헤어5.250°/제한25°, 넥타이4.170°/제한12°이며 근접·초과0이다. 애니메이션 wind anchor를 PhysBone 각도에 합치지 않았다.

**잔여 콜라이더 여유:** Head −0.02520mm로 동일, Neck −0.21934mm(이전 실행−0.15656mm)이다. 약0.063mm 차이는 새로 웨이트를 줄인 chain00이 아닌 hair_tip_05의 무풍 Translation에서 발생한다. 해당 설정과 무관한 실행 간 변화 원인을 단정하지 않고, 프리팹 재구성의 작은 잔여 겹침으로 기록한다. 실제 메시 표면 전수나 SDK 접촉 허용오차의 합격 판정은 아니다.

근거: 최신 `/tmp/v348_surface_contact_review.json`, `/tmp/v348_physics_independent.json`, `/tmp/v348_physics_colliders.json`. 첫 통합 증거는 제작자가 별도 trials/technical_evidence에 보존했으며 `/tmp/v348_first_*`에도 분리했다.


## 제작 담당 반영 확인

TECH-004의 전용 scene 배치, 실제 unity_frame/time 기록, v348 물리 출처 표기를 반영했다. 재현 입력은 분할 압축과 해시 목록으로 작업 저장소에 보존한다. 최종 표면 보존 검사는 헤어 웨이트 이외 차이0, v344 대비 정적 비헤어 표면 최대0.00037mm 이내다. 위 과거 의심·첫 통합 결과는 최종 판정과 구별해 보존한다.

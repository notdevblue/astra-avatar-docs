> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v068 실제 본과 엔진 전달 검증

저장한 검수본을 다시 열어 `RUNTIME_BONES.json`으로102개 본의 부모·rest 행렬·웨이트 정점 수·제약을 추출했다. 아래는 실제 파일 상태다. Unity/VRChat에서 실행된 결과가 아니다.

## 회전 보조본

모든 아래 제약은 Blender Copy Rotation, LOCAL→LOCAL, REPLACE, XYZ 활성, influence0.5다. 제약16개 중14개 본에 실제 웨이트가 있고 shoulder_support 좌우는0개다. 웨이트 수는 raw 정점이며 본별로 중복될 수 있다.

| 본 (좌/우) | 부모 | 따라갈 본 | 웨이트 정점 좌/우 |
|---|---|---|---:|
| volume_index_02 | index_01 | index_02 |86/86|
| volume_middle_02 | middle_01 | middle_02 |75/81|
| volume_ring_02 | ring_01 | ring_02 |69/84|
| volume_little_02 | little_01 | little_02 |101/81|
| elbow_support | upper_arm | forearm |49/44|
| shoulder_support | clavicle | upper_arm |0/0|
| knee_support | thigh | shin |193/190|
| hip_support | hips | thigh |162/165|

이름에는 `.L`/`.R` 접미사가 붙는다. 손의 PIP8본만 이번에 추가했고 기존 팔꿈치·무릎·고관절 구조는 유지했다. 중지 보조본은 머리·꼬리를 함께 좌우 바깥 X방향1.5mm 옮긴 중심을 보존해야 한다. 눈에 보이는 본 방향만 맞추고 회전 중심이나 rest 기준을 버리면 결과가 달라진다.

VRChat의 회전 제약으로 옮길 때 같은 이름의 컴포넌트를 넣는 것만으로 일치했다고 보지 않는다. Humanoid 기본 뼈 매핑에 보조본을 대체 입력하지 않고 원래 계층을 유지한다. 재생 시 목표/소유자의 부모 공간·rest offset·회전 혼합·평가 순서를 맞춘 뒤, 각 축과 복합 회전에서 Blender 평가 정점과 실제 엔진 정점을 비교한다. 단일축 각도의 절반과 복합 회전의 보간은 구분해야 한다. [VRC Rotation Constraint](https://creators.vrchat.com/common-components/constraints/vrc-rotation-constraint/).

현재 Shape Key0, armature driver0이다. 시험했던 자동 코트 자세키는 포함하지 않았다. Blender의 제약은 FBX 저장만으로 동등한 VRChat 실행이 보장되지 않는다.

## 머리카락·옷자락 준비 상태

- 머리카락: head 아래 hair_anchor_00–05, 각 anchor 아래 hair_XX_01→02. anchor6개+실제 웨이트를 가진12본.
- 코트: hips 아래 coat_anchor_front/back.L/R, 각 anchor 아래 coat_front/back_01→02. anchor4개+웨이트8본.
- 넥타이: chest 아래 tie_anchor→tie_01→02→03. anchor1개+웨이트3본.

계층·웨이트가 있다는 것과 자연스러운 물리 움직임은 별개다. 이번에는 PhysBone 컴포넌트·콜라이더 반경·각도 제한·바람을 Unity에서 실행하지 않았다. 자락은 앉기/다리 들기에서 기본 변형을 먼저 검증하고 그 위에 보조 움직임을 더해야 한다. 머리카락·넥타이는 얼굴/목/가슴 충돌, 급회전 후 복원, 과도한 뒤집힘을 확인해야 한다. 손가락·주 관절 보조본을 물리로 대체하지 않는다. [VRChat PhysBones](https://creators.vrchat.com/common-components/physbones/).

## 내보내기 전에 남은 확인

1. 현재 본과 가중치, rest pose, 원래 Humanoid 경로를 실제 FBX 재임포트 결과와 대응한다. 팔 명령 +120°는 원래 A포즈에 더한 회전이며 해부학적 외전120°와 같지 않다.
2. [v065](../v065_frozen_triangles/REVIEW.md)의 전체 삼각형 고정 사본은 하체 회귀로 제외했다. 실제 FBX/Unity 면 인덱스를 읽어 v068의11소매 대각선과 채택 무릎을 각각 확인해야 한다. 전체 자동 삼각 분할을 검증 없이 덮어씌우지 않는다.
3. 손 기본 펼침·주먹·검지·브이·엄지 및 실제 Humanoid muscle 입력을 검사한다. 현재 랜덤 local 회전 검사를 실제 VRChat 손 입력 검사라고 부르지 않는다.
4. 최대4웨이트를 유지한다. 프로젝트/클라이언트 설정이 확인되지 않은 Unlimited 영향 수를 전제로 수정하지 않는다.
5. 새 면적 경고뿐 아니라 깊은 접힘·실루엣·음영·접촉을 비교한다. 자연스러운 관절 접촉을 무조건 제거하지 않는다.

위 엔진 단계는 전달 요건이다. 사용자가 우선 요청한 큰 겨드랑이 형상과 손 잔여 문제의 해결을 완료했다고 대신하지 않는다.

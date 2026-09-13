> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v349 기술 조사 — 남은 접촉과 월드 바람 사전 구현

2026-09-13, independent technical_review. 자산/프로젝트 코드 수정·Unity 실행 0. SDK 3.10.5 DLL과 현재 프리팹을 읽고, v348 보존 데이터에서 추가 광선 검사를 /tmp에 실행했다. 아래 수정안은 아직 구현/합격이 아니다.

## TECH-010 — Head/Neck 작은 음수의 원인 분리

### 확정한 설정과 발생 조건

기준 파일: `unity_vrchat_avatar/Assets/BiancaNprFinal/Bianca_v348_FinalReview.prefab`, `avatar_modeling/v348_final_npr_review/physics/Final_MOTION.json.gz`, `technical_evidence/v348_physics_colliders.json`.

| 항목 | Head | Neck |
|---|---|---|
| 최종 최소 재구성 여유 | −0.02519977mm | −0.21934478mm |
| 본 | hair_01_02 | hair_tip_05 |
| global frame | 3731 | 1369 |
| 구간 | Wind 131/60=2.1833초 | Translation 169/60=2.8167초, 무풍 |
| 원래 최소 | +2.33979mm | −0.14282mm |
| 현 collider local radius | .00043000013 | .00019000004 |
| 현 collider local height | .0012500006 | .00066000025 |
| 실제 크기 | radius43mm/height125mm | radius19mm/height66mm |
| 재구성 중심 | head 회전 기준 (0,64.999576,0)mm | chest 회전 기준 (0,129.999675,0)mm |

두 collider는 Capsule, insideBounds=false, bonesAsSpheres=false, component transform localScale1이나 상위 포함 lossyScale100이다. Head 자식 위치 y=.0006499958, Neck 자식 위치 y=.00024999655. 헤어 PB 반경 .00004000001×100=4mm, radiusCurve 비어 있음. 현 6체인은 pull .25/spring .28/stiffness .30/immobile .10, angle25°다.

`allowCollision:0`는 SDK enum AdvancedBool.False다. 그러나 **명시 collider 목록이 꺼진다는 뜻이 아니다**. SDK `PhysBoneManager.UpdateCollidersForChain`은 `comp.colliders`를 먼저 등록하며 permission 분기를 거치지 않는다. 별도 global collision records는 `IsCollisionAllowed`를 통과해야 한다. Head/Neck/Chest/양 어깨 5개는 명시 목록에 존재한다. 따라서 allowCollision을1로 바꿔 이 잔여를 고친다는 접근은 원인에 맞지 않으며, 불필요하게 글로벌 상호작용을 넓힌다.

### SDK에서 확인한 잔여 가능 경로, 아직 확정하지 않은 것

SDK 실제 `/tmp/v349_dynamics.il`:

- `PhysBoneJob.SolveCollisions` 약33332행: 현재 collider마다 침투 보정 후 `ClampGlobalBoneLength`를 적용한다(약33590행).
- caller 약32728행의 SolveCollisions 이후에도 시간 비율에 따른 위치 lerp와 길이 재정규화가 있다.
- 여러 collider를 한 번씩 순서대로 처리한다. 앞 collider에서 확보한 여유가 뒤 보정/길이 제약/시간 보간 이후 정확히 보존된다는 수학적 보장이 아니다.
- 따라서 post-SDK transform의 −0.025/−0.219mm를 곧바로 콜라이더 누락, Animator 덮어쓰기, 잘못된 허용오차라고 확정할 수 없다. 관측 위치는 실제 최종 transform이며 SDK 내부 solver 접촉점과 동일 데이터가 아니다.
- Neck은 현재 분석에서 neck-to-chest를 프리팹 rest로 재구성했다. 정확한 매 프레임 Neck collider localToWorld/shape.position/rotation/scale을 기록해야 이 근사를 제거할 수 있다.
- 이전 Final과 현재 Final의 Neck 차이는 chain00 웨이트 변경이 아닌 chain05 무풍 구간이다. 웨이트는 본 물리 입력을 바꾸지 않는다. 시뮬레이션 시간 위상/콜라이더 interpolation/초기화 실행 차이와 원래 근사 오차가 후보이지, 어느 하나를 이번 읽기 검사만으로 확정하지 않았다.

### 최소 수정/진단 순서

1. v349 사본에서 먼저 collider transform 전체, parent head/neck/chest pose, root rotation, `Time.frameCount/deltaTime/fixedTime/fixedDeltaTime`, PB 실제 본 끝점을 **같은 OnFrameCompleteLate** 시점에 기록한다. 기존 재구성과 실제 행렬 재구성 차이를 기록한다. 체인 radiusCurve/scale도 함께 기록한다.
2. 같은 프리팹으로 최소 3회 반복(새 inactive instance→설정→활성,90warmup). 체인01·05에 집중하되 기존 7구간 입력을 그대로 유지한다. Original/Final 섞은 실행순서 영향도 1회 비교한다. 60Hz 동일 캡처가 SDK 내부 fixed-time 위상까지 동일함을 뜻하지 않는다.
3. **0.3mm 등방 capsule padding** 후보는 타당하다. 실제 보호 표면에 보수적 여유를 추가하는 것으로 설명한다. scale100에서 `radius += .000003f; height += .000006f;`이다. 반경만 늘리면 capsule 중앙 선분이 짧아져 맨 끝 캡이 확장되지 않는다. 실제 적용은 `padLocal=.0003f/max(abs(lossyScale))`로 계산하고 uniform scale을 확인한다. 중심/축/본 반경/limit/weights는 유지한다.
4. 패딩본에서 다시 실제 실행한다. **원래 보호 capsule에 대한 여유와 새 패딩 capsule에 대한 여유를 둘 다** 기록한다. 후자가 소량 음수여도 전자는 양수로 확보될 수 있다. 콜라이더 간격으로 얼굴/카라 메시 무관통을 대체하지 않는다. 0.3mm가 성공하는지는 아직 미실행이다.
5. 패딩으로 원형/움직임이 악화할 때만 chain01 wind1.9→1.6, chain05 immobile .1→.2/.3처럼 **한 변수씩** 비교한다. chain05 문제에 바람 전체 감소는 무관하다. 0.3/0.5mm padding의 외형 이탈·정지 rest 변화도 검수해야 한다.

## TECH-011 — 헤어–얼굴/코트의 실제 위치와 노출 선별

기존 최종 표면 자료의 80시각(Translation20/Turn20/Wind40)에서 새 pair instance624회, 이 중 헤어–셔츠/넥타이–셔츠 새 교차는0이다. 이번에는 헤어–얼굴/코트 pair의 교차선 내부3점에 앞/뒤/좌/우/뒤사선2/위,7방향 광선을 실제 최종 mesh BVH로 발사했다. 두 면 모두 opaque/double-sided 형상으로 간주하며 0.1mm내 첫 표면일 때 노출 후보로 분류했다.

- 624회 중 **163회/40고유 pair**가 노출 후보. 전부 hair inventory component10→coat. Translation24/Turn4/Wind135회.
- 이번 7방향 선별에서 얼굴 노출 후보0. 이는 모든 카메라/모든 원래 얼굴 접촉/알파/백페이스 설정을 검사한 결과가 아니다.
- 가장 긴 새 얼굴 pair: Wind2.5초, hair16439(poly14308)↔face1898(poly3029),4.323mm 교차선. world 약(-.037,.874,-.015)m, 머리 옆/귀뒤 안쪽. 원래 face1898에는 hair16448가 이미 접촉했고 두 교차선 사이 거리0이다. 이번 광선에서14~112mm 앞에 다른 표면이 있어 내부 경계로 분류한다.
- 가장 긴 새 코트 pair: Wind3.0초 hair16182↔Coat422,4.453mm. 원래 같은 pair 거리1.530mm, 가장 가까운 원래 접촉선0.701mm. 뒤목 기존 접촉 경계 확장이다. 대표 선 내부5점은 뒤방향에서도0.319~2.873mm 가려졌다.
- **실제 확대 우선 대상**: Wind2.0초 hair15703(poly13931)↔Coat3296(poly1870/body_R_back),교차선3.189mm. world x−.0240~−.0242,y.8306~.8314,z−.0311~−.0280m. `(.3,0,-1)` 뒤사선에서 교차선 후반이 표면에 노출될 수 있다. Wind3.5초 같은pair3.180mm,Wind6.5초hair15673↔Coat3343도 후보다.

현재 480×600 `Final_Wind_HeadBack_0120.png`와 `Original_Wind_HeadBack_0120.png`, 최종0150을 직접 열었다. 머리카락과 카라가 닿는 상태는 보이지만, 위3mm 선이 육안에서 결함인지는 작은 해상도로 확정하지 못했다. 해당 세계좌표 중심 orthographicSize .015~.025m, 앞선 뒤사선/좌뒤 2방향,1024², 실제원형재질+단색별도 진단 렌더가 필요하다. 원래 접촉과 비교하고 가닥이 코트 밖으로 다시 튀어나오는지 연속영상으로 확인한다.

최소 수정 후보는 **component10/chain00의 카라 인접 가닥만**이다. 현 component10 변경은 기존 head 영향에서 분담한 TailExtension을 .4배로 제한했고 하단 hair합계 약.229~.237(예16182)이다. 얼굴 안쪽16439는 .103~.138이다. `.24→.18/.12` local weight cap 또는 현재변위의.75/.5배 예측으로 노출40쌍을 먼저 줄여보고, 실제 SDK 재실행한다. 이미 원래 상태에서 관통한 가닥 전체를 0으로 줄여도 원래 접촉은 남는다. 그 경우 원형정점/자세보정 국소0.2~0.5mm와 코트 surface clearance를 따로 비교해야 하며 전체 hair projection/정수리/앞가닥을 이동하지 않는다. 새 메시/리메시 불필요.

출력: `/tmp/v349_contact_spatial.json`, `/tmp/v349_contact_weights.json`, `/tmp/v349_contact_visibility.json`, `/tmp/v349_exposed_contacts.json`. 교차선 길이는 관통 깊이가 아니다. 163은 결함 개수도 아니며 material alpha/전시각 노출 판정이 아니다.

## TECH-012 — 월드 바람 지원 범위와 실제 가능한 구현

공식 PhysBone은 Pull/Spring/Gravity/Immobile 등을 제공하고, 아바타에서 force 속성을 계속 애니메이션하는 방식은 지원하지 않는다. 해당 값은 초기화 시 설정한다. 현재 SDK3.10.5 공개 PhysBone 필드/DLL에도 WindZone/풍속벡터 소비 경로가 없었다. 현 `BiancaWind`는 `hair_wind_*`와 `tie_anchor`를 회전하는 authored loop를 BlendTree로 섞은 것이다. 임의 월드의 Unity WindZone을 자동으로 읽는 아바타 일반 기능이라고 표현할 근거가 없다. [VRChat PhysBones](https://creators.vrchat.com/common-components/physbones/)

아바타는 일반 커스텀 MonoBehaviour/Udon을 실행하는 위치가 아니다. 제공할 avatar에는 SDK ContactReceiver/Animator/기존 PB만 저장하고, 테스트 runner는 Editor 전용으로 분리한다. [허용 아바타 컴포넌트](https://creators.vrchat.com/avatars/whitelisted-avatar-components/whitelisted-avatar-components/)

**권장 호환 입력:** 협력 월드가 명시 태그 `BiancaWorldWind`의 ContactSender를 제공하고, 아바타의 작은 ContactReceiver가 float `WorldWind`를0..1로 받는다. Receiver는 인접 신호에 따라 Animator 파라미터를 쓸 수 있고, World sender는 Avatar content type을 지정할 수 있다. 이름이 같은 임의 월드 바람과 자동호환되는 것이 아니라 문서화된 프로토콜이다. [VRChat Contacts](https://creators.vrchat.com/common-components/contacts/)

### 기존 수동/ambient 유지와 합성

기존 `BiancaWind=M` 기본.35를 그대로 둔다. 새로운 `WorldWind=W` 기본0, 내부전용 `WorldWindFloor=G` 기본.7을 추가한다. 이 두 내부 파라미터는 사용자가 조작하는 기존 float를 덮어쓰지 않는다. 외부풍 없는 월드에서 기존결과를 정확히 보존하는 것이 우선이다.

권장 스칼라 clip weight: `E=M+W*max(0,.7-M)`.

| M | W0 | W.5 | W1 |
|---|---:|---:|---:|
|0|0|.35|.7|
|.35|.35|.525|.7|
|.7|.7|.7|.7|
|1|1|1|1|

Unity Animator에서 임의 두 float의 연속 max를 코드식처럼 직접 계산한다고 가정하지 않는다. 다음 **같은 기존 wind layer 내부 중첩 1D**로 구현 가능하다:

- ManualTree: parameter BiancaWind, Calm threshold0/Breeze1.
- ConstantFloorTree: parameter WorldWindFloor(default.7, 외부/driver 변경없음), Calm0/Breeze1.
- FloorTree: parameter BiancaWind, ConstantFloorTree threshold.7/Breeze1. .7아래는 첫 자식으로 clamp된다.
- OuterTree: parameter WorldWind, ManualTree0/FloorTree1.

W1은 max(M,.7),중간W는 추가 영향만 선형 증가한다. E=max(M,W)와는 다른 명시한 규칙이다. 새 additive layer로 동일 앵커를 중복회전하지 않는다. 원래 wind mask만 재사용하고 pose measurements mask·관절 state·FX 연결은 보존한다. 같은 Calm/Breeze 길이10초/loop/phase를 유지한다. scalar clip contribution은 위 식이나, 중첩 quaternion interpolation 및 animation phase 차이는 실제 Unity pose 비교로 확인해야 한다. [Unity 1D blending](https://docs.unity3d.com/2022.3/Documentation/Manual/BlendTree-1DBlending.html)

### 현재 SDK 실제 필드와 사전 시험

네임스페이스 `VRC.SDK3.Dynamics.Contact.Components`, types `VRCContactSender`,`VRCContactReceiver`; 공통 enum `VRC.Dynamics.ContactBase.ShapeType.Sphere/Box`, ReceiverType `VRC.Dynamics.ContactReceiver.ReceiverType.Constant/Proximity`.

Receiver 설정: 새 자식의 world scale1 확인(100배rig에직결하면 radius를100으로나눔), radius.02m, collisionTags=[BiancaWorldWind], parameter=WorldWind, receiverType=Constant로 첫0/1검증, localOnly=false, allowSelf=false/allowOthers=true(명시된 external 신호). Proximity는 첫연결확인후 추가한다. WorldSender: avatar밖 scale1 node, Sphere radius.5m 또는 Box size1m, 같은tag, contentTypes=DynamicsUsageFlags.Avatar. 무관한 태그/멀리 떨어진sender 음성대조 필수.

**SDK 에디터 구분:** DynamicsComponent.Usage getter는 공개지만 setter는 internal이고, avatar 프로젝트 기본Usage는 Avatar이다. 단순 avatar밖 GameObject를 만들었다고 `Usage==World`가 되는지 반드시 확인한다. 실제 world SDK preview에서 확인하는 것이 최선이다. 현 avatar 프로젝트 안에서 실행해야 한다면 테스트에서만 reflection으로 sender의 internal Usage를World로 설정한 뒤 활성화하고, 이것을 `SDK World usage simulated in avatar editor`로 기록한다. 해당 reflection/runner를 전달 avatar나 production world에 넣지 않는다. production world SDK는 자체 초기화로 설정한다.

ContactReceiver에는 public `paramValue`,`paramAccess:IAnimParameterAccess`가 있고 SDK가 paramAccess가 있으면 setter를 호출한다. 에디터에서 paramValue만변하고 Animator float가 안바뀌면 자동바인딩누락이다. 이를 접촉실패와 분리한다. 시험용 IAnimParameterAccess adapter가 필요하면 아래처럼 공개interface를 사용하되 adapter 사용을 기록하고 PC runtime binding확정으로 확대하지 않는다:

```csharp
sealed class WindParam : VRC.SDKBase.IAnimParameterAccess {
    public Animator animator; public string name="WorldWind";
    public float floatVal { get=>animator.GetFloat(name); set=>animator.SetFloat(name,value); }
    public int intVal { get=>Mathf.RoundToInt(floatVal); set=>floatVal=value; }
    public bool boolVal { get=>floatVal>.5f; set=>floatVal=value?1:0; }
}
```

최소 시험은 사본 avatar inactive에서 receiver/필드/필요adapter를 모두설정→활성→90warmup. sender를10초진입/10초퇴장, M=0/.35/.7/1각각 실행. **WorldWind를 runner가 SetFloat하지 않고** 접촉을 실제로 발생시킨다. OnFrameCompleteLate에서 sender/receiverUsage,receiver.paramValue,Animator.GetFloat(WorldWind),M,월드거리,anchorquaternion,본tip/실제mesh를 기록한다. WorldWind가1→0으로복귀하고 M.35가 유지되는지 확인한다. sampler는최소1프레임의Contact→Animator평가시간차를허용하되 실제delay를 기록한다. no-sender 및 wrong-tag는 W0, World sender제거/비활성뒤W0, 동일태그2sender중1개만퇴장때W유지,머리·넥타이 자연복귀까지 포함한다. Proximity 전환시선형풍속센서라고단정하지말고실제거리→param곡선을측정한다.

공식문서는 Contacts/PhysBones의 에디터 구동을 지원한다. 이 사전시험은 실제 SDK 접촉/Animator/PB 결합이며, 실제 월드에서의 권한·원격동기화·클라이언트 Avatar FX 바인딩은 PC 단계에 남는다. [공식 디버깅](https://creators.vrchat.com/avatars/avatar-components/debugging-avatar-components/)

글로벌 PhysBoneCollider를 월드에서 이동해 머리를 미는 대안도 최신 SDK에 있지만 현재 PB allowCollision=False이므로 그대로는 수신되지 않고, 촉각충돌과 바람의 구별/권한/표면눌림 문제가 있다. 이번 목표의 최소안은 Contact 프로토콜이며 강제로글로벌충돌을활성화하는것은권장하지않는다.

## 재현 파일/명령과 수정 담당용 실행 관문

- builder source: `avatar_modeling/v347_secondary_motion/MacSecondaryMotionViews.cs`의 AddTieWind/AmplifyHairWind/ApplyWeights; 현Final은 Safe tie x0/z8,chain01gain1.9,나머지2.25,TailClearance weights.
- latest runtime: `avatar_modeling/v348_final_npr_review/MacFinalSecondaryAudit.cs`; 실행entry `MacFinalSecondaryAuditViews.Begin()` 또는 `BeginFinal()`. 기존O/A경로에덮어쓰므로 **v349로복제/새SessionState/출력경로변경후** 제작담당만호출. 검토자는호출하지않았다.
- rawgeometry: `.venv/v344_texture_rig/*SOURCE.json`; metadata `avatar_modeling/v344_texture_rig_retest/*TRIANGLE_MAP.json`; latestbins `v348_final_npr_review/physics/mesh_samples`. rawgzip/archives복원은기존`restore_evidence.py`가프로젝트쓰기하므로필요시제작담당이처리.이번캐시는존재했다.
- 최신사본간회귀: 기존v348gray/basecolor/UV/본등hash보존, 새수정요소명시, 고정동일시각의중립및7구간구동,동일.7wind재실행,새WorldWind진입별도.

```sh
.venv/mac_python/bin/python /tmp/v349_contact_spatial.py
'[local home]/Library/Application Support/Steam/steamapps/common/Blender/Blender.app/Contents/MacOS/Blender' -b -t 2 --python /tmp/v349_contact_visibility.py
'[local home]/Library/Application Support/Steam/steamapps/common/Blender/Blender.app/Contents/MacOS/Blender' -b -t 2 --python /tmp/v349_exposed_contacts.py
```

SDK반복읽기는 Unity내장mono로ikdasm.exe를직접실행한다. `bin/ikdasm` 래퍼는빌드서버경로를하드코딩해이Mac에서실패했다.

```sh
'/Applications/Unity/Hub/Editor/2022.3.22f1/Unity.app/Contents/MonoBleedingEdge/bin/mono' '/Applications/Unity/Hub/Editor/2022.3.22f1/Unity.app/Contents/MonoBleedingEdge/lib/mono/4.5/ikdasm.exe' unity_vrchat_avatar/Packages/com.vrchat.base/Runtime/VRCSDK/Plugins/VRC.Dynamics.dll > /tmp/v349_dynamics.il
```

현재검토의실행완료는 read-only SDK분석/80시각노출선별/대표사진대조뿐이다. padding/contact/blendtree/더넓은각도영상은 아직사전구현안이며 완료항목에 넣지 않는다.

## TECH-013 — HEAD_SIDE 흰점 투영 진단의 부호 오류 (후속)

제작자의 `avatar_modeling/v349_remaining_npr/probe_hair_pixels.py`를 읽고 SIDE ray의 화면가로→world z 부호 오류를 발견했다. 카메라 target+(2,0,0), forward(-1,0,0), up(0,1,0)이므로 camera.right=up×forward=(0,0,+1)이다. 화면 x가 커질수록 world z가 커져야 하나 원래 probe는 음수로 계산하여 좌우 반대편을 조회했다.

수정식: `point=target+[2, (.5-(y+.5)/1050)*.32, ((x+.5)/850-.5)*.32*850/1050]`. 세로의 top-origin/half-pixel은 유지한다. det>0은 이 actual mesh에서 outward normal과 일치(174 BodyHair27813삼각형positive,135negative의원래변형예외존재). 전역 det부호를반대로바꿀근거는없다.

/tmp사본에서174이미지(396,759)만재계산했다. first front-facing hit는 **face8414**, world(0.0125693545,0.8406821733,0.0074824628)m,bary(.1567616573,.5038696723,.3393686705). 그뒤 반대face15566, hair15573. 이전hair2496/darktexture표본과렌더밝기차는shader결함증거가아니다. 실제피부가헤어사이에서보이는것인지분리한다. 원래probe는미채택진단으로보존하고제작담당이수정한다. 출력 `/tmp/v349_probe_corrected.py`, `/tmp/v349_HAIR_WHITE_CORRECTED.json`.

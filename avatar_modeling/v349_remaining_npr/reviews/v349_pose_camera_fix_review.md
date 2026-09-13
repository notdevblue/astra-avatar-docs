> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v349 WorldContact pose/camera 수정 재실행 독립 검토

판정: **새 실행에서 정상 pose와 카메라 안의 아바타가 확인됐으며 실제 SDK Contact 입력/수동값 보존/60Hz 기록도 통과했습니다.** 기존 빈 영상은 실패 기록으로 보존해야 합니다. 이 결과는 Editor World Usage 어댑터 시험이며 PC VRChat 실제 월드/클라이언트 실행 검증은 아닙니다.

## 실행 증거

`world_wind/pose_camera_fix/RUN.json`, SHA256 `66262a099f148ccb4b3143cf266208fe323ed0977d130e9faea9779466264c0d`.

- error=null,4800기록, frame0–4799 연속.
- 실제 Unity frame 간격은전부1, 기록 delta_time은전부0.0166666675초.
- root_position 전부(0,0,0), head.y 전부0.8449998m.
- 모든 본 position/quaternion은유한합니다.
- primary/second/wrong tag Sender가4800기록 모두World, Receiver는Avatar입니다.
- manual0/.35/.7/1은각1200기록에서정확히보존됐고오차0입니다.
- receiver SDK값과 Animator WorldWind float의 차이는모든기록0입니다.

## 실제 접촉 구간

아래7구간을각manual값4개에서반복한총28구간모두예상응답과일치합니다. frame은각1200프레임시나리오내상대값이며양끝포함입니다.

|frame|실제입력|receiver/WorldWind|
|---|---|---:|
|0–119|sender없음|0|
|120–359|맞는tag sender1개|1|
|360–479|맞는tag sender2개중복|1|
|480–539|첫sender이탈,두번째만잔류|1|
|540–599|모두이탈|0|
|600–719|잘못된tag만접촉|0|
|720–1199|모두없음|0|

단순 모의 파라미터 대입이 아니라 실제 SDK contact paramAccess setter가 Animator float에 연결된 구조입니다. Editor 어댑터는 sender 활성화 후 Usage를World로 다시보장하는reflection과IAnimParameterAccess 연결이며, 이 한계는이전설명그대로유지합니다.

## 중요한 타이밍 해석 — 1프레임 지연

**receiver값과 Animator float는 같은기록에서일치하지만 실제 변형은그다음프레임에반영됩니다.** manual0에서:

- frame120:receiver가1,Animator WorldWind도1이지만바람anchor는아직calm.
- frame121:실제anchor회전발생.
- frame540:receiver/WorldWind가0으로복귀하지만anchor는앞프레임바람을유지.
- frame541:anchor가calm으로복귀.

SDK Contact완료 단계가 해당프레임Animator평가보다뒤이므로다음Animator평가에반영되는1프레임(약16.7ms) 지연과일치합니다. “같은프레임에옷/헤어가변형된다” 또는 “0ms입력지연”이라는주장은안됩니다. PhysBone끝점의물리감쇠는이anchor입력복귀와또다릅니다.

## 수동강도 보존과 실제 anchor 반응

manual0에서world없을때바람anchor회전은수치오차수준(<0.000003도),world접촉중최대7.2676도,이탈후calm복귀입니다. manual.35는이탈뒤기존ambient .35가남으므로완전정지가정상목표가아닙니다.

효과적강도가같은조건의실제anchor quaternion대조:

- manual.7 대 manual.35+World=1:접촉첫프레임제외최대0.01518도차이.
- 첫접촉프레임만최대2.66666도차이지만이는위1프레임평가지연으로설명됩니다. 동일프레임정확일치합격으로보고하면안됩니다.
- manual.7에서worldon/off의같은10초clip위상비교최대0.01276도.
- manual1에서같은비교최대0.01829도.

기존clip의부동소수점위상차수준잔차는있으나중복sender로더세게증폭되거나,강한manual이worldfloor로낮아지는결과는없습니다. 정확한수식동일성은controller구조,실제회전반응은이비교로구분합니다.

## 이미지 확인

PNG120장을검사했으며각사진의배경과다른픽셀은180,210–180,496개(600×800중약37.5%)입니다.120장의hash도모두다릅니다. 이전world_usage의120장단일배경과달리모델이실제로촬영됐습니다.

대표frame0000/0240/0480/0720의4개PNG를직접열어머리·넥타이·상체가화면에있음을확인했습니다. 현재캡처는manual.35시나리오의1200프레임중10프레임마다1장인**6fps/20초**입니다. 실제SDK기록60Hz와영상6fps를구분해야합니다. 120장만으로빠른움직임의30/60fps연속시각품질을검수했다고하면안됩니다.

정상pose복원으로카메라실패가해결된것은확인됐지만,원래초기Animator root motion과humanoid body재구성중어느단계가−0.614m이동을만들었는지를새RUN이완전히분리한것은아닙니다. 수정된테스트에서는기준HumanPose를매프레임복원해이동을방지합니다. 기존실패의원인은“초기기준pose복원이없는하네스와고정카메라가런타임위치를놓침”까지확정한범위를유지합니다.

## 남는 시험 경계

- 실제VRChat클라이언트/네트워크/호환월드에서의수신과동작은미검증.
- 임의Unity WindZone 자동감지시험아님; 전용tag Contact프로토콜.
- 이번실행의모든몸/헤어표면관통전수검사는아님.
- 첫90워밍업은코드에있지만RUN에는워밍업행이없어그구간의각프레임원시값을사후확인한것은아님.
- 새RUN은root/head기록을추가했으나초기평가전후hips/bodyPosition기록은없습니다. 이전원인내부단계분리에는별도소규모프로브가필요합니다.

독립산출물 `/tmp/v349_pose_camera_fix_review.py/json`. 프로젝트변경·Unity bridge호출없이코드/원시JSON/PNG만읽었습니다.

> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v353 Cloth 정상 스킨 추종 원인 분리

Cloth 품질 검수 전에 전체 고정 Cloth가 원래 스킨을 따라가는지 확인해야 한다. Unity 2022.3 공식 문서는 maxDistance=0 정점을 시뮬레이션하지 않고 스킨 위치에 두도록 설명한다. 따라서 전체 입자가 중립에 남는 현재 시험을 자연스러운 자켓 처짐으로 해석할 수 없다. [Unity maxDistance](https://docs.unity3d.com/2022.3/Documentation/ScriptReference/ClothSkinningCoefficient-maxDistance.html).

기존 all-pin, 자세 유지 동안 Animator만 끈 시험, 기존 메시 좌표·바인드포즈를 대응 변환하여 렌더러 스케일1로 만든 시험, 항상 켜진 카메라의 시험을 분리했다. 마지막 visible 시험은 두 Cloth 후보 각각 1736입자×66표본이 정확히0 이동이며, 1800프레임 모두 실제 카메라 ON/렌더러 가시성 true다. 사용자 장면 복원 equal=true. 따라서 카메라 가시성이나 렌더러 스케일 하나만으로 설명되지 않는다.

다음 사본은 수면 임계값만0, 모든 maxDistance만0.00001m, 두 조건을 함께 바꾼 대조다. 0.01mm 허용치는 수치상 활동성 확인용이며 옷 처짐을 해결한 최종값이 아니다. 기존 기하·UV·웨이트는 동일한 unit-control 자산을 참조한다. 실제 실행·재로드·가시성 및 native 입자/스킨 위치로 판정한다. 실제 wake 실행은 세 조건 각각1800프레임을 완료했지만 네 번째 EPSILON_NOSLEEP443프레임 후SDK콜백중단으로 전체실패다. 장면복원은일치했다. 완전고정/수면0고정은runtime frame0대비입자이동0, epsilon은최대117.5165mm이동했다. 초기화 이전 좌표와의7.5073mm차이는runtime움직임으로계산하지않는다.

PhysX 문서는 고정 입자와 이동 제약을 구분한다. 이를 근거로 전부 고정인 조건의 활동성을 별도 진단하지만, Unity 내부 최적화가 원인이라고 확정하지 않았다. [PhysX Cloth](https://nvidiagameworks.github.io/PhysX/3.3/PhysXGuide/Manual/Cloth.html).

PC VRChat 허용 구성요소에는 Cloth가 포함된다. 지원 여부와 실제 아바타에서의 동작·성능은 별도 검증이다. [VRChat 허용 구성요소](https://creators.vrchat.com/avatars/whitelisted-avatar-components/whitelisted-avatar-components/). 커스텀 감사 스크립트는 Editor 검수용이며 아바타 런타임 구현으로 세지 않는다.


## 동일 프레임 스킨과의 비교

실제 SDK 완료 콜백에서 보이지 않는 임시 SkinnedMeshRenderer에 **같은 기존 메시·현재 본 배열·변환·현재 셰이프 값**을 연결해 BakeMesh했다. 실제 Cloth를 끄거나 입자를 리셋하지 않았다. 두 조건 각각1800프레임/66표본을 완료하고 장면복원도일치했다. 최대차이는둘다14.0631mm이며, 참조렌더러에서만셰이프를모두0으로둬도같았다. 따라서 이결과를셰이프삭제의근거로사용하지않는다. 원래32보정키보존.

지정 maxDistance0.01mm보다차이가크므로움직이기시작했다는이유만으로자켓처짐을합격시키지않는다. 다음대조는기존형상/스킨을유지하면서늘어남·굽힘강성/감쇠/테더제약만해제하는경우와,held pose 동안Animator만꺼서갱신순서를분리하는경우다. 검수용조건이며실제옷을강성0으로납품한다는뜻이아니다.

과거 Cloth/BlendShape 문제 보고는 현재 버전의 재현 증거가 아니다. 공식 검색 결과에 과거 변형 이슈 및 5.3.7에서 수정된 충돌 기록이 표시됐지만 2022.3.22f1의 미지원으로 일반화하지 않는다. [과거 BlendShape 변형 이슈](https://issuetracker.unity3d.com/issues/increasing-blend-shapes-value-to-100-the-cloth-component-distorts), [과거 충돌 수정 기록](https://issuetracker.unity3d.com/issues/crash-when-changing-blendshape-value-on-gameobject-using-cloth).

이번 직접 열기는 이전 변형 링크가 리디렉션 오류, 충돌 링크가 이전 데이터베이스 이전 안내의 Page not found를 반환했다. 따라서 과거 기록은 검색 결과 수준의 참고만 했으며 현재 제약 판정은 실제 Unity 대조에 둔다.

후속 실제 완료: Soft대조1800프레임/66표본은최대10.8476mm잔여, epsilon+Animator고정hold1800프레임/66표본도최대14.0631mm잔여. 둘다DONE성공·장면복원일치이나스킨추종합격이아니다. 강성완화나Animator홀드만으로해결됐다고보지않는다.

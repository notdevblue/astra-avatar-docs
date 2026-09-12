# v002 SiuSiu — Windows / iPhone 11 설정

`SiuSiu_iPhone_VMagicMirror_v002.vrm`을 Windows로 복사해 VMagicMirror에서 불러온다. 기존 v001 대신 이 파일을 선택한다. VRM 0.x이며 실행에 Unity는 필요 없다.

v001에서 비었던 21개를 추가해 52개 모두 실제 변형을 연결한 개인 사용용 **검수본**이다. 원본 제작자는 ADPX이며 구매 모델의 사용 조건이 유지된다. 원본 전용 페이셜 확장을 이식한 모델은 아니다.

1. iPhone과 Windows를 같은 LAN에 연결하고 iFacialMocap을 연다. `Upper` / `Lower`를 켠다.
2. VMagicMirror의 `Ex Tracker`에서 외부 추적을 켠다.
3. `Connect to App`에서 `iFacialMocap`을 선택한다. iPhone 앱에 표시된 **iPhone IP**를 넣고 `Connect`를 누른다.
4. **`Use Perfect Sync`와 `Apply LipSync using External Tracker Data`를 모두 켠다.** 입 추적 옵션이 꺼져 있으면 새 입·턱 표정도 사용되지 않는다.
5. 편한 무표정으로 정면을 보고 `Calibrate Face Pose`를 실행한다.
6. 볼 부풀림 → 입 전체 좌우 → 위/아래 입술 → 턱 좌우 → 입 벌렸다 다물기를 확인한다. 작은 코/입술의 말기·압축은 미세하게 보일 수 있다.

현재 파일은 Mac의 UniVRM에서 재수입과 표정 검사를 마쳤고, 실제 Windows+iPhone 연결은 미검증이다. 정상적으로 움직이지 않을 때 원인이 모델·설정·송수신 중 어느 것인지 아직 단정하지 않는다.

**남은 조정:** MouthClose가 JawOpen보다 강해지면 입선이 뾰족하게 뒤집힐 수 있다. 두 값을 자동 제한하는 처리는 포함하지 않았다. 입 다물기 품질까지 완성했다고 보지 않는다. 강한 웃음+벌림+혀 조합도 과장된다. [25조합 실패 영역 사진](v002_PERFECT_SYNC_REVIEW.md).

재질은 v001의 MToon 그대로다. 원래 lilToon 반사와 눈썹 스텐실 차이는 유지된다.

[공식 iFacialMocap 연결 안내](https://malaybaku.github.io/VMagicMirror/en/docs/external_tracker_ifacialmocap/) · [공식 Perfect Sync 안내](https://malaybaku.github.io/VMagicMirror/en/tips/perfect_sync/).

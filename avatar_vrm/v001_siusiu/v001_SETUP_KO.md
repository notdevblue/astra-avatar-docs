# v001 SiuSiu — iPhone 11 / iFacialMocap / VMagicMirror

`SiuSiu_iPhone_VMagicMirror_v001.vrm`을 Windows로 복사해 VMagicMirror에서 불러오세요. Unity 설치는 필요 없습니다. VRM 0.x 형식입니다.

이 파일은 **개인 사용용 검수본**입니다. ARKit 표정 이름 52개를 등록했지만 실제 변형은 31개이며 나머지 21개는 비어 있습니다. 52종 완전 대응 모델이 아닙니다. 원본 제작자는 ADPX이며 구매 모델의 사용 조건이 그대로 적용됩니다.

## 연결

1. Windows와 iPhone을 같은 공유기/LAN에 연결합니다.
2. iPhone에서 iFacialMocap을 열고 카메라와 로컬 네트워크 접근을 허용합니다. `Upper`, `Lower`를 켭니다.
3. VMagicMirror에서 VRM 파일을 불러옵니다.
4. `Ex Tracker` → `Enable External Tracker`를 켭니다.
5. `Connect to App`에서 `iFacialMocap`을 선택하고 iPhone 앱 화면에 표시되는 **iPhone의 IP 주소**를 입력한 뒤 `Connect`를 누릅니다.
6. `Use Perfect Sync`와 `Apply LipSync using External Tracker Data`를 켭니다. 후자는 마이크 립싱크 대신 iPhone의 입 움직임을 사용합니다.
7. 정면을 보고 편안한 무표정으로 `Calibrate Face Pose`를 실행합니다. 얼굴을 고정할 필요는 없고 캘리브레이션 순간에만 정면을 봅니다.
8. 양쪽 눈 감기 → 한쪽씩 윙크 → 좌우/상하 보기 → 눈썹 올리기 → 입 벌리기 → 웃기 순으로 확인합니다.

iFacialMocap의 Windows 수신 프로그램은 함께 실행하지 않습니다. VMagicMirror가 직접 연결합니다. 연결이 안 되면 iPhone의 로컬 네트워크 권한과 Windows의 VMagicMirror 방화벽 허용을 확인합니다. 방화벽 전체를 끌 필요는 없습니다.

출처: [VMagicMirror 공식 연결 안내](https://malaybaku.github.io/VMagicMirror/en/docs/external_tracker_ifacialmocap/) · [외부 추적 설정](https://malaybaku.github.io/VMagicMirror/en/docs/external_tracker/).

## 이번 표정 범위

- 눈: 좌우 독립 깜빡임, 좁힘/크게 뜨기, 상하좌우 시선. 시선은 원래 눈 본의 회전을 셰이프 키에 구웠습니다.
- 눈썹: 안쪽 올리기, 양쪽 내리기/바깥 올리기. 원본 키를 조합한 근사이며 앞머리에 가릴 수 있습니다.
- 입: 벌리기, 둥글게/오므리기, 양쪽 웃음/슬픔/늘림, 혀 표정. 원본 애니메이션의 연속 제어와 동일하지 않습니다. 혀 원본 모양은 어둡고 강한 복합 입력에서는 모양이 과장됩니다.
- 볼 좁힘은 아래 눈꺼풀 키를 이용한 약한 근사입니다.
- 기본 VRM A/I/U/E/O, Blink, 감정 프리셋도 포함합니다.
- 볼 부풀리기, 코 찡그리기, 턱 전후/좌우, 입 전체 좌우, 입술 말기·다물기·위/아래 입술 독립 제어 등 21개는 미지원입니다. 이름만 등록했고 잘못된 움직임을 배정하지 않았습니다.

## 셰이더와 검증 한계

lilToon을 표준 VRM의 MToon으로 변환했습니다. 원래 기본색 텍스처는 유지하지만 곱셈 MatCap, 두 번째 MatCap, 눈썹이 앞머리 위에 보이는 스텐실 효과는 재현되지 않습니다. 원본보다 머리/옷 반사가 약하고 눈썹이 가려질 수 있습니다. 원래 재질이 그대로 들어 있는 파일이 아닙니다.

UniVRM에서 파일 재수입, Humanoid, 표정 바인딩/변형/중립 복귀 및 사진 확인을 완료했습니다. **Windows VMagicMirror와 실제 iPhone 송수신은 아직 시험하지 못했습니다.** 머리 SpringBone은 보수적인 뒤/옆머리 4체인만 설정했으며 원래 PhysBone 전체의 자동 변환이나 물리 품질 검증은 아닙니다.

[전후 사진과 상세 검증](v001_VRM_REVIEW.md)


원본 모델·변환 VRM·Unity 패키지·코드·원시검증은 이 문서 저장소에 포함하지 않습니다. VRM 링크는 권한이 있는 비공개 작업 저장소에서만 열립니다.

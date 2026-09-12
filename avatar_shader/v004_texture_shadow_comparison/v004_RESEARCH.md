> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v004 고정 그림자와 2D 셰이더 조사

## 일반 모델도 텍스처에 그림자를 그리는가

스타일에 따라 다르다. 물리 기반 PBR의 base color/albedo는 조명과 AO를 분리하는 것이 원칙이다. Adobe PBR Guide는 base color에 조명 정보와 ambient occlusion을 넣지 않는 방향을 설명한다. [Adobe PBR Guide Part 2](https://www.adobe.com/learn/substance-3d-designer/web/the-pbr-guide-part-2?learnIn=1)

애니메이션풍 툰 재질에서는 작가가 지정한 밝은 색과 어두운 색/텍스처로 형태를 설계한다. MToon은 Lit Color와 Shade Color를 따로 두며 같은 텍스처를 양쪽에 사용할 수도 있다. 따라서 모든 칠해진 명암을 결함으로 볼 수 없다. 다만 고정된 큰 그림자는 실제 광원 방향이 바뀌어도 그대로 남는다. [VRM MToon](https://vrm.dev/en/univrm/shaders/shader_mtoon/)

lilToon의 Shadow Blur를 낮추면 애니메이션처럼 경계가 선명해진다. Shadow Strength Mask는 얼굴 등 특정 영역에서 동적 그림자를 약하게 할 수 있다. 이 기능은 base color에 이미 그려진 얼룩을 지우지는 않는다. [lilToon 그림자 설정](https://lilxyzw.github.io/lilToon/ja_JP/color/shadow.html)

## 이번 비교 설계

- A_CURRENT: v343 텍스처, 기존 셰이더.
- B_TOON: 같은 텍스처와 메시. 그림자 흐림 0.55 → 0.08, 강도 0.5 → 0.38, 얼굴 전용 강도 마스크 35/255. AsUnlit 0.28, 최소 밝기 0.32. MatCap 추가 없음.
- C_LESS_BAKED: B와 같은 셰이더, 코트 앞 4패널만 큰 고정 명암을 덜 그린 대안 텍스처. 단추·포켓·봉제선 유지. 전체 의상 탈조명 완료본이 아니다.
- COLOR_ONLY: 동적 명암과 MatCap 등을 끈 진단. 이 상태에 남는 명암은 주로 기본 텍스처와 색 레이어에서 기인한다. 외곽선/기하 구조까지 제거한 진단은 아니다.
- S_SIUSIU: 설치된 SiuSiu 원래 재질과 BlazerOnly 애니메이션 상태. 별도 디자인이므로 같은 재질 설정의 절대 품질 우열로 해석하지 않는다.

실제 SiuSiu 비교에서 얼굴 색면은 단순하고 눈/외곽선 및 헤어의 밝은 띠가 강하다. Bianca는 기본 텍스처에 세밀한 머릿결과 옷 명암이 더 많이 그려져 있다. 이 차이는 셰이더 값을 복사하는 것만으로 없어지지 않는다. 세밀한 머릿결을 일괄 삭제하면 사용자가 요구한 디테일 보존에 어긋나므로 이번에는 별도 사본의 조명 반응을 비교한다.

광원 좌/우, 정면/측면/뒤, 따뜻한 색/차가운 색/주광 없음으로 렌더한다. 모든 비교는 Unity의 실제 재질 렌더이며, 생성 이미지를 모델 적용 사진으로 쓰지 않는다.

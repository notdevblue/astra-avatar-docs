> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v344 전체 129본 점검표

[전체 관절 검토 보고서](v344_WORK_REVIEW.md) · [사진 갤러리](v344_GALLERY.md)

Blender 원본의 본 이름·부모·변형 여부·영향 정점과 Unity의 실제 움직임을 대조했습니다. 영향 정점은 Blender 원래 정점 기준입니다. 회전은 이번 전체 표본에서 최초 상태 대비 로컬 회전의 최대값이며, 정상 가동범위나 권장 각도가 아닙니다. 끝 본·앵커는 부모를 따라 위치가 움직여도 자기 로컬 회전이 0일 수 있습니다. CTRL 본은 Blender 제어용이므로 Unity Humanoid 본처럼 직접 회전하지 않는 것을 결함으로 세지 않았습니다.

51개 Humanoid 매핑과 매핑된 86개 회전 채널을 검사했습니다. 129본 전체가 Blender/Unity에 같은 이름으로 존재하며 부모 누락·순환·길이 0·유효하지 않은 Blender 드라이버는 없습니다. 표면 품질 합격과는 별개의 구조 검사입니다.

## 중심·목·머리

| 본 | 부모 | Humanoid | 영향 정점 Body / Coat | 최대 로컬 회전° |
|---|---|---|---:|---:|
| `root` | `—` | 보조/제어 | 0 / 0 | 0.00 |
| `hips` | `root` | Hips | 1395 / 458 | 37.17 |
| `spine` | `hips` | Spine | 1116 / 838 | 32.00 |
| `chest` | `spine` | Chest | 1444 / 759 | 33.66 |
| `neck` | `chest` | Neck | 942 / 201 | 32.00 |
| `head` | `neck` | Head | 20896 / 0 | 32.04 |

## 머리 보조

| 본 | 부모 | Humanoid | 영향 정점 Body / Coat | 최대 로컬 회전° |
|---|---|---|---:|---:|
| `hair_wind_00` | `head` | 보조/제어 | 0 / 0 | 2.95 |
| `hair_anchor_00` | `hair_wind_00` | 보조/제어 | 0 / 0 | 0.00 |
| `hair_00_01` | `hair_anchor_00` | 보조/제어 | 2135 / 0 | 5.32 |
| `hair_00_02` | `hair_00_01` | 보조/제어 | 1311 / 0 | 2.23 |
| `hair_tip_00` | `hair_00_02` | 보조/제어 | 0 / 0 | 0.00 |
| `hair_wind_01` | `head` | 보조/제어 | 0 / 0 | 2.41 |
| `hair_anchor_01` | `hair_wind_01` | 보조/제어 | 0 / 0 | 0.00 |
| `hair_01_01` | `hair_anchor_01` | 보조/제어 | 1946 / 0 | 5.85 |
| `hair_01_02` | `hair_01_01` | 보조/제어 | 1554 / 0 | 2.12 |
| `hair_tip_01` | `hair_01_02` | 보조/제어 | 0 / 0 | 0.00 |
| `hair_wind_02` | `head` | 보조/제어 | 0 / 0 | 2.63 |
| `hair_anchor_02` | `hair_wind_02` | 보조/제어 | 0 / 0 | 0.00 |
| `hair_02_01` | `hair_anchor_02` | 보조/제어 | 3521 / 0 | 6.56 |
| `hair_02_02` | `hair_02_01` | 보조/제어 | 2079 / 0 | 2.37 |
| `hair_tip_02` | `hair_02_02` | 보조/제어 | 0 / 0 | 0.00 |
| `hair_wind_03` | `head` | 보조/제어 | 0 / 0 | 2.14 |
| `hair_anchor_03` | `hair_wind_03` | 보조/제어 | 0 / 0 | 0.00 |
| `hair_03_01` | `hair_anchor_03` | 보조/제어 | 2613 / 0 | 5.93 |
| `hair_03_02` | `hair_03_01` | 보조/제어 | 1210 / 0 | 2.07 |
| `hair_tip_03` | `hair_03_02` | 보조/제어 | 0 / 0 | 0.00 |
| `hair_wind_04` | `head` | 보조/제어 | 0 / 0 | 2.69 |
| `hair_anchor_04` | `hair_wind_04` | 보조/제어 | 0 / 0 | 0.00 |
| `hair_04_01` | `hair_anchor_04` | 보조/제어 | 1543 / 0 | 5.81 |
| `hair_04_02` | `hair_04_01` | 보조/제어 | 606 / 0 | 1.90 |
| `hair_tip_04` | `hair_04_02` | 보조/제어 | 0 / 0 | 0.00 |
| `hair_wind_05` | `head` | 보조/제어 | 0 / 0 | 3.00 |
| `hair_anchor_05` | `hair_wind_05` | 보조/제어 | 0 / 0 | 0.00 |
| `hair_05_01` | `hair_anchor_05` | 보조/제어 | 1953 / 0 | 5.42 |
| `hair_05_02` | `hair_05_01` | 보조/제어 | 796 / 0 | 6.83 |
| `hair_tip_05` | `hair_05_02` | 보조/제어 | 0 / 0 | 0.00 |

## 어깨·팔·손목

| 본 | 부모 | Humanoid | 영향 정점 Body / Coat | 최대 로컬 회전° |
|---|---|---|---:|---:|
| `clavicle.L` | `chest` | LeftShoulder | 0 / 296 | 24.00 |
| `upper_arm.L` | `clavicle.L` | LeftUpperArm | 0 / 419 | 128.24 |
| `forearm.L` | `upper_arm.L` | LeftLowerArm | 371 / 122 | 155.34 |
| `hand.L` | `forearm.L` | LeftHand | 568 / 0 | 65.61 |
| `cuff_support.L` | `forearm.L` | 보조/제어 | 357 / 46 | 42.65 |
| `elbow_support.L` | `upper_arm.L` | 보조/제어 | 0 / 49 | 77.67 |
| `shoulder_support.L` | `clavicle.L` | 보조/제어 | 0 / 0 | 64.12 |
| `CTRL_upper_arm.L` | `clavicle.L` | 보조/제어 | 0 / 0 | 0.00 |
| `clavicle.R` | `chest` | RightShoulder | 0 / 287 | 24.00 |
| `upper_arm.R` | `clavicle.R` | RightUpperArm | 0 / 415 | 128.24 |
| `forearm.R` | `upper_arm.R` | RightLowerArm | 332 / 120 | 155.34 |
| `hand.R` | `forearm.R` | RightHand | 581 / 0 | 65.61 |
| `cuff_support.R` | `forearm.R` | 보조/제어 | 321 / 46 | 42.65 |
| `elbow_support.R` | `upper_arm.R` | 보조/제어 | 0 / 44 | 77.67 |
| `shoulder_support.R` | `clavicle.R` | 보조/제어 | 0 / 0 | 64.12 |
| `CTRL_upper_arm.R` | `clavicle.R` | 보조/제어 | 0 / 0 | 0.00 |
| `CTRL_clavicle.L` | `chest` | 보조/제어 | 0 / 0 | 0.00 |
| `CTRL_clavicle.R` | `chest` | 보조/제어 | 0 / 0 | 0.00 |

## 손가락·두께 보조

| 본 | 부모 | Humanoid | 영향 정점 Body / Coat | 최대 로컬 회전° |
|---|---|---|---:|---:|
| `thumb_01.L` | `hand.L` | Left Thumb Proximal | 237 / 0 | 51.39 |
| `thumb_02.L` | `thumb_01.L` | Left Thumb Intermediate | 167 / 0 | 61.65 |
| `thumb_03.L` | `thumb_02.L` | Left Thumb Distal | 148 / 0 | 62.58 |
| `index_01.L` | `hand.L` | Left Index Proximal | 192 / 0 | 83.06 |
| `index_02.L` | `index_01.L` | Left Index Intermediate | 73 / 0 | 73.96 |
| `index_03.L` | `index_02.L` | Left Index Distal | 102 / 0 | 81.35 |
| `volume_index_02.L` | `index_01.L` | 보조/제어 | 84 / 0 | 36.98 |
| `middle_01.L` | `hand.L` | Left Middle Proximal | 271 / 0 | 83.25 |
| `middle_02.L` | `middle_01.L` | Left Middle Intermediate | 97 / 0 | 55.29 |
| `middle_03.L` | `middle_02.L` | Left Middle Distal | 110 / 0 | 81.36 |
| `volume_middle_02.L` | `middle_01.L` | 보조/제어 | 75 / 0 | 27.65 |
| `ring_01.L` | `hand.L` | Left Ring Proximal | 218 / 0 | 83.21 |
| `ring_02.L` | `ring_01.L` | Left Ring Intermediate | 113 / 0 | 57.93 |
| `ring_03.L` | `ring_02.L` | Left Ring Distal | 113 / 0 | 81.40 |
| `volume_ring_02.L` | `ring_01.L` | 보조/제어 | 69 / 0 | 28.96 |
| `little_01.L` | `hand.L` | Left Little Proximal | 194 / 0 | 65.78 |
| `little_02.L` | `little_01.L` | Left Little Intermediate | 115 / 0 | 76.84 |
| `little_03.L` | `little_02.L` | Left Little Distal | 116 / 0 | 81.15 |
| `volume_little_02.L` | `little_01.L` | 보조/제어 | 101 / 0 | 38.42 |
| `thumb_01.R` | `hand.R` | Right Thumb Proximal | 249 / 0 | 52.41 |
| `thumb_02.R` | `thumb_01.R` | Right Thumb Intermediate | 176 / 0 | 61.65 |
| `thumb_03.R` | `thumb_02.R` | Right Thumb Distal | 145 / 0 | 62.58 |
| `index_01.R` | `hand.R` | Right Index Proximal | 190 / 0 | 83.06 |
| `index_02.R` | `index_01.R` | Right Index Intermediate | 85 / 0 | 73.96 |
| `index_03.R` | `index_02.R` | Right Index Distal | 100 / 0 | 81.35 |
| `volume_index_02.R` | `index_01.R` | 보조/제어 | 86 / 0 | 36.98 |
| `middle_01.R` | `hand.R` | Right Middle Proximal | 278 / 0 | 83.32 |
| `middle_02.R` | `middle_01.R` | Right Middle Intermediate | 93 / 0 | 55.30 |
| `middle_03.R` | `middle_02.R` | Right Middle Distal | 101 / 0 | 81.46 |
| `volume_middle_02.R` | `middle_01.R` | 보조/제어 | 81 / 0 | 27.65 |
| `ring_01.R` | `hand.R` | Right Ring Proximal | 246 / 0 | 82.31 |
| `ring_02.R` | `ring_01.R` | Right Ring Intermediate | 97 / 0 | 60.42 |
| `ring_03.R` | `ring_02.R` | Right Ring Distal | 106 / 0 | 81.49 |
| `volume_ring_02.R` | `ring_01.R` | 보조/제어 | 84 / 0 | 30.21 |
| `little_01.R` | `hand.R` | Right Little Proximal | 183 / 0 | 65.87 |
| `little_02.R` | `little_01.R` | Right Little Intermediate | 99 / 0 | 76.79 |
| `little_03.R` | `little_02.R` | Right Little Distal | 101 / 0 | 81.11 |
| `volume_little_02.R` | `little_01.R` | 보조/제어 | 81 / 0 | 38.39 |

## 넥타이 보조

| 본 | 부모 | Humanoid | 영향 정점 Body / Coat | 최대 로컬 회전° |
|---|---|---|---:|---:|
| `tie_anchor` | `chest` | 보조/제어 | 0 / 0 | 0.00 |
| `tie_01` | `tie_anchor` | 보조/제어 | 105 / 0 | 1.53 |
| `tie_02` | `tie_01` | 보조/제어 | 58 / 0 | 0.82 |
| `tie_03` | `tie_02` | 보조/제어 | 57 / 0 | 1.06 |
| `tie_tip` | `tie_03` | 보조/제어 | 0 / 0 | 0.00 |

## 고관절·다리·발

| 본 | 부모 | Humanoid | 영향 정점 Body / Coat | 최대 로컬 회전° |
|---|---|---|---:|---:|
| `thigh.L` | `hips` | LeftUpperLeg | 566 / 0 | 109.17 |
| `shin.L` | `thigh.L` | LeftLowerLeg | 575 / 0 | 144.12 |
| `foot.L` | `shin.L` | LeftFoot | 937 / 0 | 42.49 |
| `toes.L` | `foot.L` | LeftToes | 712 / 0 | 40.00 |
| `knee_support.L` | `thigh.L` | 보조/제어 | 193 / 0 | 72.06 |
| `thigh.R` | `hips` | RightUpperLeg | 575 / 0 | 109.17 |
| `shin.R` | `thigh.R` | RightLowerLeg | 586 / 0 | 144.12 |
| `foot.R` | `shin.R` | RightFoot | 921 / 0 | 41.75 |
| `toes.R` | `foot.R` | RightToes | 698 / 0 | 40.00 |
| `knee_support.R` | `thigh.R` | 보조/제어 | 190 / 0 | 72.06 |
| `hip_support.L` | `hips` | 보조/제어 | 162 / 0 | 54.58 |
| `hip_support.R` | `hips` | 보조/제어 | 165 / 0 | 54.58 |

## 코트 자락 보조

| 본 | 부모 | Humanoid | 영향 정점 Body / Coat | 최대 로컬 회전° |
|---|---|---|---:|---:|
| `coat_anchor_front.L` | `hips` | 보조/제어 | 0 / 0 | 0.00 |
| `coat_wind_front.L` | `coat_anchor_front.L` | 보조/제어 | 0 / 0 | 1.42 |
| `coat_front_01.L` | `coat_wind_front.L` | 보조/제어 | 0 / 241 | 1.15 |
| `coat_front_02.L` | `coat_front_01.L` | 보조/제어 | 0 / 102 | 0.97 |
| `coat_front_tip.L` | `coat_front_02.L` | 보조/제어 | 0 / 0 | 0.00 |
| `coat_anchor_front.R` | `hips` | 보조/제어 | 0 / 0 | 0.00 |
| `coat_wind_front.R` | `coat_anchor_front.R` | 보조/제어 | 0 / 0 | 2.00 |
| `coat_front_01.R` | `coat_wind_front.R` | 보조/제어 | 0 / 241 | 0.79 |
| `coat_front_02.R` | `coat_front_01.R` | 보조/제어 | 0 / 102 | 0.97 |
| `coat_front_tip.R` | `coat_front_02.R` | 보조/제어 | 0 / 0 | 0.00 |
| `coat_anchor_back.L` | `hips` | 보조/제어 | 0 / 0 | 0.00 |
| `coat_wind_back.L` | `coat_anchor_back.L` | 보조/제어 | 0 / 0 | 1.65 |
| `coat_back_01.L` | `coat_wind_back.L` | 보조/제어 | 0 / 291 | 1.38 |
| `coat_back_02.L` | `coat_back_01.L` | 보조/제어 | 0 / 162 | 0.72 |
| `coat_back_tip.L` | `coat_back_02.L` | 보조/제어 | 0 / 0 | 0.00 |
| `coat_anchor_back.R` | `hips` | 보조/제어 | 0 / 0 | 0.00 |
| `coat_wind_back.R` | `coat_anchor_back.R` | 보조/제어 | 0 / 0 | 1.87 |
| `coat_back_01.R` | `coat_wind_back.R` | 보조/제어 | 0 / 295 | 1.52 |
| `coat_back_02.R` | `coat_back_01.R` | 보조/제어 | 0 / 162 | 0.71 |
| `coat_back_tip.R` | `coat_back_02.R` | 보조/제어 | 0 / 0 | 0.00 |

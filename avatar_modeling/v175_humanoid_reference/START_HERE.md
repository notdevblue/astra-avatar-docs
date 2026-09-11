> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v175 — 실제 Humanoid 기준 자세·손가락 매핑 수정

검토 Unity 프리팹 `Assets/BiancaIntegration/BiancaTPoseReview.prefab`. 원래 FBX와 메시를 유지하고 별도 Avatar 에셋만 사용한다. 메시 재생성, 원래 본rest/웨이트 수정은 없다.

기존 설정은 팔을 내린 원래 rest를 Humanoid의 T-pose 기준으로 사용했다. `isValid=true`였어도 SDK idle에서 양팔이 등 뒤로 모였다. Unity의 Enforce T-Pose 경로로 임시 인스턴스의 기준을 계산해 별도 Avatar를 만들었고, 실제 사진에서 팔을 자연스럽게 내리는 대기 자세로 바뀌었다.

또한 손가락 humanName은 `LeftIndexDistal`이 아니라 Unity가 요구하는 `Left Index Distal`이어야 했다. 기존 정의51개를 실제 매핑51개로 오해한 부분을 바로잡았다. 전체 HumanTrait 이름에 맞춰 정규화했다. 최종 정렬 오류469.3885→0, 유효한 Humanoid. v176 BUILD에서 **51개 모두 실제 GetBoneTransform 결과를 확인**했다.

첫20자세 `TPOSE_INITIAL_NO_FINGERS_CHECK.json`은 손가락 수정 전 자료다. `TPOSE_INTEGRATED_CHECK.json`도 당시 자료이며 최신 손가락 통과 근거로 사용하지 않는다. 최신 전체 통합은 아래 연속 검사와 후속 v176이다.

## 손가락 수정 후 연속 검증

실제 Unity IK·머리/목/몸통/손가락 근육 애니메이션·24자동보정·30VRC제약·11PhysBones·바람·루트 이동, ON/OFF각1200프레임. 121프레임씩 전체 표면을 기록했다. 메시 좌표 finite, 기록한 관절 위치 ON/OFF 차이0, 보정 신호도 ON/OFF 동일이다. 따라서 물리 효과를 자세 오차와 구분할 수 있다.

v173 계산으로 보정 지연이3프레임에서 **2프레임**으로 줄었다. 60Hz 기준33ms에 해당하지만 실제 VRChat 지연을 측정한 것은 아니다. 2프레임 정렬 후 최대 키 오차7.4248e-6(0.00074248%). 정렬 전 최대0.03885는 연속 지연 영향이다.

코트 물리 ON/OFF최대 차이37.21mm, BodyHair9.05mm. 정지 복귀 오차 코트0, BodyHair약0.00012mm. 코트 면적비 경고0. BodyHair13사건은 모두 동일한 극소 삼각형3207이며, 원래 면적1.82e-12m²의 거의 일직선인 세 정점이다. 큰 머리카락 부피가13곳 파손된 뜻이 아니다. 상세 위치·면적·가중치는 v176과 `CONTINUOUS_AREA_DETAIL.json`에 남긴다.

실제 대기 앞, 연속600뒤 사진을 열어 확인했다. 앞90/겨드랑이의 원래 불규칙한 접힘은 여전히 남아 있다. 모든 관절의 외형 문제가 해결됐다는 의미는 아니다. 원시 `.venv/bianca_continuous_tpose`, 수치 `CONTINUOUS_SUMMARY.json`.

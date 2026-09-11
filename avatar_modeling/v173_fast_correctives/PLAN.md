> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# 자동 자세 보정 지연 감소

v174 첫 연속 검사에서 기존 자동 보정은 정확히 **3프레임 지연**이었다.3프레임을 맞춰 비교하면 최대오차 약8.5e-6이지만 같은 프레임의 키 차이는최대0.0713였다.60fps 가정50ms이며 빠른 팔 움직임에서 보정 형상이 늦을 수 있다.

등급을 맞추기 위해 기능을 없애지 않는다. 기존24키 수식과 센서 배치를 유지하면서 중간 Gate 파라미터 레이어를 제거하고, 마지막 ShapeKey 트리에 가중 계산을 직접 중첩한다. 회전거리/행렬 측정→최종키 두단계로 줄인다. 바람은 별도3번째레이어,전상태WriteDefaultsON. `BiancaFastCorrectiveReview.prefab` 별도 후보에서32/192자세 및 연속검사를 거쳐 선택한다. 원래프리팹은 비교 기준으로 보존한다.

근거: Unity [Direct Blend Tree](https://docs.unity.cn/Manual/BlendTree-DirectBlending.html), [2D Blend Tree](https://docs.unity.cn/Documentation/Manual/BlendTree-2DBlending.html), v167 검증된 원래 수식과 v174 실제 프레임별기대/실측키. 새로운 키를 만들거나 기존 메시를 변경하는 작업이 아니다.

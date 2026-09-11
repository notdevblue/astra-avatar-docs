> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v177 — lilToon Soft 검토 재질 채택

Standard/Soft/Flat 세 설정을 대기·팔 앞·팔 뒤·팔 위12자세, 밝은/어두운 조명에서 비교했다. 같은 메시와 같은 텍스처를 사용했고 원본 `avator_references/ref_char.png`를 다시 열어 색과 명암을 비교했다.

검수용 Standard에서 깊게 보이던 음영이 lilToon에서 원화에 가까운 색으로 보였다. 밝은 환경의 Soft/Flat 차이는 작지만 실제 픽셀 차이가 있고(앞12만화소, 최대51/255), Soft는 약한 입체 음영을 유지해 선택했다. 그림자를 완전히 끄는 Flat은 비교용이다. 대기 앞·앞팔 뒤·약한 조명 사진을 실제 확인했다.

기하·본·웨이트·UV·텍스처는 바꾸지 않았다. MaterialIntegration 검사12자세 최대 자동 보정 오차7.56979e-6. 기존 목/어깨 접힘을 재질로 해결했다고 해석하지 않는다. v180 검토 패키지에 Soft가 포함된다.

공식 lilToon2.3.4, LightMinLimit0.2/Max1, AsUnlit0.1, ShadowStrength0.5. 출처와 설정 의도는 PLAN.md. Unity 원시 사진 `.venv/bianca_material_review`, 대표 사진은 v180에 모았다.

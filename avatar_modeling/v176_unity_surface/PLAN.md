> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v172 면 연결의 Unity 통합과 잔여 미세 면 진단

536자세에서 검증한 기존 코트 edge 두 곳만 Unity의 보존 웨이트 메시 사본에 반영한다. 재수입으로 작은 웨이트를 다시 잃지 않도록 정점/전체UV/노멀/모든 shape frame/bindpose/웨이트는 유지하고 삼각형 연결만 바꾼다. 별도 프리팹으로 저장한다.

v175 연속 검사 BodyHair13면적 경고는 모두 동일한 삼각형3207(정점6743/6745/6744)이다. 위치는 머리카락 끝 부근, OFF면적0.00011–0.00051mm²로 거의 선이다. 실제 원형/웨이트와 인접면을 확인해 큰 헤어 손상과 구분한다. 임계값을 바꿔 경고를 숨기지 않는다. 필요한 경우 기존 국소 면 연결만 정리한다.

참고 [Unity Mesh triangles](https://docs.unity3d.com/2022.3/Documentation/ScriptReference/Mesh-triangles.html), [Blender Rotate Edge](https://docs.blender.org/manual/en/4.1/modeling/meshes/editing/edge/rotate_edge.html). 원래 Blender v167과 v172를 직접 비교한 패치 자료를 생성하고 Unity 사본에서 속성 보존을 전수 검사한다.

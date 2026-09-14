> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v354 작업 기록

2026-09-15 시작. [승인된 계획](v354_WORK_PLAN.md).

- 계획 갱신 및 기존 v352 기준 파일 확인 착수.
- 독립 검토: 넥타이 PAD1 최소 이식 범위, 어깨 V7 재로드와 Ribbon 접촉 실패 원인 조사.
- Blender 현재 열린 파일이 폐기된 v353 눈꺼풀 복원 사본임을 Computer Use로 확인. 아직 기준 파일로 교체하지 않았다.
- 아직 새 모델/물리 적용 완료 또는 최종 품질 통과를 주장하지 않는다.

## P0 기준 파일 확인

- v352 전달 기록의 34개 항목 SHA가 모두 일치했다.
- Blender Computer Use로 v352 원본을 실제 열고 v354_BASELINE_REVIEW 사본을 저장·재열었다. 형상/UV/노멀/웨이트/키/본/재질/이미지 지문 동일을 확인했다.
- 재열린 사본의 실제 전신 카메라 뷰를 직접 확인했다. 기존 의복 주름과 신발 부품 표현이 있는 기준 외형이다. 세부 Unity 구도 확인은 다음 검수에서 수행한다.
- Unity 2022.3.22f1 / Windows64 대상, Play OFF와 깨끗한 현재 장면을 확인했다. 아직 Unity 기준 프리팹을 새 장면에 배치하지 않았다.

## 사전 자료 재확인

- [VRChat PhysBones 공식 문서](https://creators.vrchat.com/common-components/physbones/): 체인 위치별 곡선, 중력·복귀·충돌 반경을 확인했다. 넥타이 매듭과 아래 부분의 역할을 나눠 기존 시험 후보를 검증한다.
- [Unity Prefab 저장 API](https://docs.unity3d.com/2022.3/Documentation/ScriptReference/PrefabUtility.SaveAsPrefabAsset.html): 저장 후 재로드 비교를 유지한다. 어깨 후보에서 의도한 루트 이름 변경을 전체 데이터 손실로 오인했을 가능성을 별도로 조사 중이다.

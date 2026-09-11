> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v147 — 팔 전방 거상에서 어깨가 납작해지는 원인

사용자 관찰을 v146의 실제 좌표로 확인했다. 원본 `avator_references/ref_char.png`를 다시 열어 정장 어깨선과 볼륨을 기준으로 삼았다. 이 버전은 진단이며 새 모델 채택본이 아니다.

## 확인된 원인

동일한 기존 코트 정점 집합을 기본 자세와 앞 90도에서 비교하고, 강체 회전을 제거했다. 소매산 영역의 앞뒤 두께가 기본의 **66.6%**로 줄었다. 기존 보정 키를 모두 꺼도 **66.7%**다. 따라서 이전 봉제선 보정만의 문제가 아니라, 상완과 몸통 사이 스키닝 변형 자체가 주원인이다.

소매산 중심은 앞으로 17.88mm 이동했고 최적 강체 회전은 64.6도였다. 몸통 쪽 어깨판은 중심 전방 이동 1.47mm, 회전 18.5도였다. 어깨 전체가 같은 양으로 앞으로 밀린다는 해석보다, **소매산이 많이 돌아가면서 앞뒤로 압축되는 현상**이 관찰과 더 잘 맞는다. 이 수치는 특정 정점 영역의 형상 진단이며 사람의 라운드 숄더 진단이 아니다.

Blender Preserve Volume 실험에서는 소매산 두께가 86.8%로 돌아오지만 hull 부피 비율은 1.202로 커졌다. 전체 적용하면 다른 접힘도 바뀌므로 그대로 채택하지 않는다. 기존 v122에서도 DQ의 면적·교차 부작용을 확인한 이력이 있다.

## 자료와 이번 모델에 적용할 판단

- [Skinning course](https://www.skinning.org/): 선형 블렌딩의 압축과 회전 문제를 이해하는 기준. 본의 위치가 같아도 혼합된 변환 때문에 형태가 줄 수 있다.
- [Kavan 등의 dual quaternion skinning](https://users.cs.utah.edu/~ladislav/kavan07skinning/kavan07skinning.html), [연구자 설명](https://users.cs.utah.edu/~ladislav/dq/index.html): 회전 혼합의 부피 손실을 줄이는 대안. 모든 의상 접힘·접촉을 자동으로 해결하는 방법으로 취급하지 않는다.
- [Autodesk DQ 설명](https://download.autodesk.com/global/docs/Softimage2014/en_us/userguide/files/idef_deforms_DualQuaternionSkinning.htm): 선형 방식과 부피 보존 방식의 차이. 우리 모델에서는 국소 비교 기준으로 사용했다.
- [Disney Enhanced DQ](https://media.disneyanimation.com/uploads/production/publication_asset/98/asset/dualQ.pdf), [Elastic skinning](https://media.disneyanimation.com/uploads/production/publication_asset/18/asset/elasticSkinning.pdf): 제작용 스키닝에서 부피와 접촉 처리가 함께 필요하다는 후속 조사 자료. 이 턴에서 PDF 전체를 정독하거나 영상을 시청한 것은 아니다.
- [Disney technical animation](https://disneyanimation.com/process/technical-animation/), [simulation](https://disneyanimation.com/process/simulation/): 의상 형태 보정과 시뮬레이션은 별개 검증 항목. 이번 정적 키 보정을 실시간 천 시뮬레이션 완성으로 표현하지 않는다.

확정한 실험 방향은 기존 메시·본·웨이트·원본 텍스처를 유지하고, 회전에 따라 줄어든 소매산 두께를 별도 키로 복원하는 것이다. 회전 전체를 억제하는 안도 비교하고, 겨드랑이 충돌을 함께 제한한다. 결과는 [v148](../v148_volume_trials/START_HERE.md)과 후속 버전에 기록한다.

## 재현과 한계

측정 원자료 (공유본 미포함: `VOLUME_AUDIT.json`), 본 위치 (공유본 미포함: `POSE_BONES.json`), `CAPTURE.npz`, `REGIONS.npz`, 두 실행 스크립트와 MCP 로그를 보관했다. 기본·30·60·90·120·뒤35·뒤60, 기존/기존 키 OFF/DQ/키 OFF DQ를 비교했다.

Convex hull은 열린 코트 정점 집합의 외형 대용량이며 실제 닫힌 의상이나 인체 부피가 아니다. 행렬 determinant는 LBS 변환 진단이며 실제 메시 Jacobian이 아니다. 프레임을 회전 정렬한 축별 두께와 영상 검수를 함께 사용한다. Unity/VRChat PC에서의 결과는 이 진단에 포함되지 않는다.

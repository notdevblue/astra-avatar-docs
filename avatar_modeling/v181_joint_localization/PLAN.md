> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v181 실제 Unity 경고 부위 분류와 원인 자료

v180을 보호하고 실제 SDK 자동 보정이 켜진 동일 233자세를 재현한다. 새 캐릭터 메시 생성 없이 기존 메시의 정점, 본 행렬, 웨이트, 면적, 부위 속성을 읽는다.

1. 경고 면을 Blender 원래 정점/부위와 대응하고, 큰 면과 극소면을 구분한다.
2. 앞팔 어깨 및 복합 자세 허리·바지·손의 경고를 앞/뒤/측면에서 촬영한다.
3. 실제 본 행렬로 기존 스키닝을 재구성해 오차를 검증한다. 이후 후보 웨이트를 같은 입력으로 비교할 수 있게 한다.
4. 기존 면의 국소 웨이트/연결 또는 기존 보정키 수정 후보를 별도 파일에 만들고, 변경하지 않은 자세와 무작위 자세에서 회귀를 검사한다. 좋은 수치만으로 외형 통과를 주장하지 않는다.

반복 실패했던 전역 홈 채우기와 무차별 cut을 그대로 반복하지 않는다. 원형·텍스처·기존 관절 개선을 보호한다. 실제 VRChat 실행/업로드는 하지 않는다.

## 이번에 다시 확인한 근거

- [Autodesk: Pose Space Deformation](https://www.autodesk.com/support/technical/article/caas/tsarticles/ts/5g4jy8ZLAeajfT7Qg9YG2W.html): 웨이트만으로 해결하기 어려운 부위에 자세별 보정을 사용한다. 현재 자동키 값이 정확하다는 사실과 보정 외형이 충분하다는 판단은 구분한다.
- [Autodesk: Skin weighting and deformations](https://download.autodesk.com/us/maya/2011help/files/Smooth_skinning_Skin_weighting_and_deformations.htm): 어깨를 움직이는 영향 본을 개별적으로 확인하고 웨이트를 수정한다. 이번에는 실제 문제가 난 면의 본별 분포를 기록한다.
- [Blender Armature](https://docs.blender.org/manual/en/latest/modeling/modifiers/deform/armature.html): Preserve Volume과 일반 선형 스키닝의 차이가 있다. Blender 옵션을 켜는 것만으로 Unity 해결로 간주하지 않으며 실제 Unity 행렬 결과를 기준으로 검증한다.

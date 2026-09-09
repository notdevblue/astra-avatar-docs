> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# 출처와 조사 범위

**열람일 2026-09-09 · 외부 31개 자료(YouTube 8편 포함) + 로컬 근거 3개**

공식 문서·논문·제작자 원본을 우선했다. 자료별 열람 방법과 버전은 아래에 기록한다. 검색으로 확인한 본문과 페이지 전체 직접 열람은 구분하며, 접근 오류가 난 최신 문서는 확인 가능한 공식 버전의 제한된 원리만 사용했다. 새 버전/SDK에서 달라질 수 있는 설정은 실제 구현 시 다시 확인한다.

연구 후보 선택 기준은 관절 원리, 실제 웨이트 작업, PC 런타임 호환성, 현재 모델과의 관련성이다. 유료 강좌 전체를 열람했다고 주장하지 않으며 원본 자막·영상·논문 전문을 저장소에 복제하지 않는다.

<a id="source-1"></a>

## 1. Anatomy and Physiology 2e, 9.5 Types of Body Movements

OpenStax · 2e · 열람 2026-09-09

[Anatomy and Physiology 2e, 9.5 Types of Body Movements](<https://openstax.org/books/anatomy-and-physiology-2e/pages/9-5-types-of-body-movements>)

**열람:** 본문/검색 열람

**사용 범위와 한계:** 굽힘·벌림·회내/회외·엄지 맞섬 등 운동 구분. 의학적 가동 범위 수치의 근거로 쓰지 않음.

<a id="source-2"></a>

## 2. Armature Modifier

Blender Foundation · 5.2 / latest · 열람 2026-09-09

[Armature Modifier](<https://docs.blender.org/manual/en/latest/modeling/modifiers/deform/armature.html>)

**열람:** 공식 문서 열람

**사용 범위와 한계:** Preserve Volume과 웨이트 기반 변형. Unity의 동일 계산 지원을 입증하는 출처가 아님.

<a id="source-3"></a>

## 3. Geometric Skinning with Approximate Dual Quaternion Blending

Ladislav Kavan et al. · ACM TOG 2008 · 열람 2026-09-09

[Geometric Skinning with Approximate Dual Quaternion Blending](<https://dcgi.fel.cvut.cz/publications/2008/kavan-tog-adqb/>)

**열람:** 저자 기관 논문 페이지/PDF 확인

**사용 범위와 한계:** 선형 스키닝의 회전 혼합 문제와 DQ 원리. 검색의 수집일을 논문 발행일로 쓰지 않음.

<a id="source-4"></a>

## 4. Rigify — Bone Positioning Guide

Blender Foundation · 5.2 / latest · 열람 2026-09-09

[Rigify — Bone Positioning Guide](<https://docs.blender.org/manual/en/latest/addons/rigify/bone_positioning.html>)

**열람:** 공식 문서/검색 본문 확인

**사용 범위와 한계:** 관절 중심·손가락·축 배치 원리. Rigify 전체 생성/교체를 권장하거나 실행하지 않음.

<a id="source-5"></a>

## 5. Corrective Smooth Modifier

Blender Foundation · 4.2 · 열람 2026-09-09

[Corrective Smooth Modifier](<https://docs.blender.org/manual/en/4.2/modeling/modifiers/deform/corrective_smooth.html>)

**열람:** 공식 버전 문서 확인

**사용 범위와 한계:** 포즈 후 표면 보정의 성격. 5.2 UI와 동일하다고 단정하지 않음.

<a id="source-6"></a>

## 6. Bendy Bones

Blender Foundation · 3.5 · 열람 2026-09-09

[Bendy Bones](<https://docs.blender.org/manual/en/3.5/animation/armatures/bones/properties/bendy_bones.html>)

**열람:** 공식 버전 문서 확인

**사용 범위와 한계:** 가상 세그먼트 기반 변형. FBX 실제 본 체인 자동 전환의 근거가 아님.

<a id="source-7"></a>

## 7. Shape Keys — Introduction

Blender Foundation · 3.0; 5.1 dev Workflow 검색 본문 교차 확인 · 열람 2026-09-09

[Shape Keys — Introduction](<https://docs.blender.org/manual/id/3.0/animation/shape_keys/introduction.html>)

**열람:** 공식 검색 본문 확인; latest Workflow 직접 열람 오류

**사용 범위와 한계:** 정점 위치를 저장하는 원리만 사용. 최신 UI 절차의 근거로 사용하지 않음.

<a id="source-8"></a>

## 8. FBX (Legacy)

Blender Foundation · 5.2 · 열람 2026-09-09

[FBX (Legacy)](<https://docs.blender.org/manual/en/5.2/files/import_export/fbx_legacy.html>)

**열람:** 공식 문서 확인

**사용 범위와 한계:** shape-key export 제한 문구와 설치 exporter 코드가 충돌함. source 32와 함께 실제 왕복 검증 관문으로 해석.

<a id="source-9"></a>

## 9. Importing a model with humanoid animations

Unity Technologies · 2022.3 · 열람 2026-09-09

[Importing a model with humanoid animations](<https://docs.unity3d.com/kr/2022.3/Manual/ConfiguringtheAvatar.html>)

**열람:** 공식 문서 확인

**사용 범위와 한계:** Humanoid 매핑·기준 자세·Skin Weights 설정. 매핑 성공을 변형 품질 합격으로 보지 않음.

<a id="source-10"></a>

## 10. Avatar Muscle & Settings tab

Unity Technologies · 2022.3 · 열람 2026-09-09

[Avatar Muscle & Settings tab](<https://docs.unity3d.com/2022.3/Documentation/Manual/MuscleDefinitions.html>)

**열람:** 공식 HTML 본문 확인

**사용 범위와 한계:** 근육 그룹/개별 움직임 Preview, 설정 검수. 모든 실제 관절각을 Animator에 제공한다는 뜻이 아님.

<a id="source-11"></a>

## 11. Skinned Mesh Renderer component

Unity Technologies · 2022.3 · 열람 2026-09-09

[Skinned Mesh Renderer component](<https://docs.unity3d.com/cn/2022.3/Manual/class-SkinnedMeshRenderer.html>)

**열람:** 공식 문서 확인

**사용 범위와 한계:** SkinnedMeshRenderer의 본·BlendShape·영향 제한·Bounds 등 검사 근거.

<a id="source-12"></a>

## 12. Current Unity Version

VRChat · 조사일 지원 Editor 2022.3.22f1 · 열람 2026-09-09

[Current Unity Version](<https://creators.vrchat.com/sdk/upgrade/current-unity-version/>)

**열람:** 공식 페이지 확인

**사용 범위와 한계:** 실제 프로젝트 구성 때 재확인 필요. 로컬 2022.3.62f2 시험 프로젝트와 다름.

<a id="source-13"></a>

## 13. Rig Requirements

VRChat · SDK3 기준 · 열람 2026-09-09

[Rig Requirements](<https://creators.vrchat.com/avatars/rig-requirements/>)

**열람:** 공식 페이지 확인

**사용 범위와 한계:** Humanoid 계층·어깨/목·toes, UpperChest/누락 손가락 지원. SDK2 제한과 구분.

<a id="source-14"></a>

## 14. Full-Body Tracking

VRChat · 조사일 문서 · 열람 2026-09-09

[Full-Body Tracking](<https://docs.vrchat.com/docs/full-body-tracking>)

**열람:** 공식 본문 확인

**사용 범위와 한계:** Hips와 upper-leg 시작점 관계·관절 배치·축·자세 검수. 현재 모델 원인 확정 자료는 아님.

<a id="source-15"></a>

## 15. Animator Parameters

VRChat · 조사일 문서 · 열람 2026-09-09

[Animator Parameters](<https://creators.vrchat.com/avatars/animator-parameters/>)

**열람:** 공식 파라미터 표 확인

**사용 범위와 한계:** 기본 제공 값과 의미. 모든 관절의 각도 입력은 없음. Gesture/AngularY를 오해하지 않음.

<a id="source-16"></a>

## 16. Playable Layers

VRChat · 조사일 문서 · 열람 2026-09-09

[Playable Layers](<https://creators.vrchat.com/avatars/playable-layers/>)

**열람:** 공식 본문 확인

**사용 범위와 한계:** 레이어 역할·마스크·추적과 클립의 상호작용. 현재 아바타 컨트롤러를 실행한 증거는 아님.

<a id="source-17"></a>

## 17. VRChat Constraints

VRChat · 조사일 문서 · 열람 2026-09-09

[VRChat Constraints](<https://creators.vrchat.com/common-components/constraints/>)

**열람:** 공식 본문 확인

**사용 범위와 한계:** transform 제약·공간·계층과 비용. 일반 RBF/PSD나 자동 swing–twist 구현을 보장하지 않음.

<a id="source-18"></a>

## 18. PhysBones

VRChat · 조사일 문서 · 열람 2026-09-09

[PhysBones](<https://creators.vrchat.com/common-components/physbones/>)

**열람:** 공식 본문 확인

**사용 범위와 한계:** 본 체인·충돌·제한·Constraint와의 계층 주의점. 메시 전체의 천 충돌과 구분.

<a id="source-19"></a>

## 19. Allowed Avatar Components

VRChat · 조사일 문서 · 열람 2026-09-09

[Allowed Avatar Components](<https://creators.vrchat.com/avatars/whitelisted-avatar-components/whitelisted-avatar-components/>)

**열람:** 공식 목록 확인

**사용 범위와 한계:** PC Cloth 등 허용 요소, 임의 사용자 스크립트를 일반 아바타 구동 경로로 쓰지 않음.

<a id="source-20"></a>

## 20. Avatar Performance Ranking System — PC Limits

VRChat · 페이지 표시 갱신 2026-04-21 · 열람 2026-09-09

[Avatar Performance Ranking System — PC Limits](<https://creators.vrchat.com/avatars/avatar-performance-ranking-system/>)

**열람:** 공식 웹 페이지 및 Chrome 본문/PC 표 직접 확인

**사용 범위와 한계:** PC 표만 적용. 등급 한계와 업로드 허용·실제 FPS·시각 품질은 서로 다름.

<a id="source-21"></a>

## 21. Cloth

Unity Technologies · 2022.3 · 열람 2026-09-09

[Cloth](<https://docs.unity3d.com/2022.3/Documentation/Manual/class-Cloth.html>)

**열람:** 공식 HTML 본문 확인

**사용 범위와 한계:** SkinnedMeshRenderer·고정/이동 범위·명시적 충돌체 등. 모든 월드 Collider와 자동 충돌하지 않음.

<a id="source-22"></a>

## 22. Example-based Shape Deformation

J. P. Lewis · SIGGRAPH 2014 course material · 열람 2026-09-09

[Example-based Shape Deformation](<https://www.skinning.org/example-based.pdf>)

**열람:** 강의 PDF 확인

**사용 범위와 한계:** 예제 포즈에 따른 보정 개념. VRChat에서 자유 관절각 구동이 준비됐다는 뜻이 아님.

<a id="source-23"></a>

## 23. RIGGING L3-6 : Pro Weight Painting

CGDive · 2025-04-15, 약 54분 · 열람 2026-09-09

[RIGGING L3-6 : Pro Weight Painting](<https://www.youtube.com/watch?v=rf3NDKXYpTk>)

**열람:** 원본 자동 영어 자막의 무릎/머리/목/칼라 구간 읽음

**사용 범위와 한계:** 구간과 적용 판단은 YOUTUBE_NOTES.md. 전체 자막 재배포 없음.

<a id="source-24"></a>

## 24. RIGGING L2-2 : IK, Fingers, Bone Roll

CGDive · 게시일 미확인, 약 16분 · 열람 2026-09-09

[RIGGING L2-2 : IK, Fingers, Bone Roll](<https://www.youtube.com/watch?v=QnCo0hzXKeQ>)

**열람:** 원본 자동 영어 자막 00:00–04:30 읽음

**사용 범위와 한계:** 손가락 중심과 축. 엄지 및 MCP 벌림을 별도로 해석.

<a id="source-25"></a>

## 25. 3D Character Creation for Animation - The Shoulder

Anthony Gibbs · 게시일 미확인, 약 6분 · 열람 2026-09-09

[3D Character Creation for Animation - The Shoulder](<https://www.youtube.com/watch?v=c_XM01I6ed0>)

**열람:** 원본 제공 영어 자막 전체 읽음

**사용 범위와 한계:** Bendy Bones·DQ·Smooth를 Blender 내부 사례로 분류.

<a id="source-26"></a>

## 26. 3D Character Creation for Animation - Elbow Topology

Anthony Gibbs · 게시일 미확인, 약 4분 39초 · 열람 2026-09-09

[3D Character Creation for Animation - Elbow Topology](<https://www.youtube.com/watch?v=dEvWZwRxeWQ>)

**열람:** 원본 제공 영어 자막 전체 읽음; 01:17 이후 재생 장면 표본 확인

**사용 범위와 한계:** 루프 수의 보편 규칙으로 일반화하지 않음.

<a id="source-27"></a>

## 27. Modeling For Animation 01 - TOP 5 Features of Good Animation Models!

Dikko · 게시일 미확인, 약 30분 · 열람 2026-09-09

[Modeling For Animation 01 - TOP 5 Features of Good Animation Models!](<https://www.youtube.com/watch?v=01WQlMD7dsk>)

**열람:** 원본 자동 영어 자막 약 09:00–19:45 등 관련 구간 읽음

**사용 범위와 한계:** 새 모델 설계 맥락. 현재는 국소 진단 원리만 채택.

<a id="source-28"></a>

## 28. Rigging hanging equipment in Blender

Pierrick Picaut · 게시일 미확인 · 열람 2026-09-09

[Rigging hanging equipment in Blender](<https://www.youtube.com/watch?v=Vg2oS1P9m2o>)

**열람:** 원본 제공 영어 자막 00:00–05:17 읽음

**사용 범위와 한계:** 매달린 부품의 역할 분리. 실제 천 물리와 다름.

<a id="source-29"></a>

## 29. RIGGING L3-3 : Improved Leg Rig

CGDive · 게시일 미확인, 약 59분 · 열람 2026-09-09

[RIGGING L3-3 : Improved Leg Rig](<https://www.youtube.com/watch?v=ErR4RysRy0g>)

**열람:** 원본 자동 영어 자막 00:00–02:40, 03:43–09:40 읽음

**사용 범위와 한계:** 발 피벗은 제작용 제어 리그. VRChat IK 대체안으로 쓰지 않음.

<a id="source-30"></a>

## 30. This Blender Trick Makes Weight Painting EASY

Pierrick Picaut · 게시일 미확인, 약 12분 · 열람 2026-09-09

[This Blender Trick Makes Weight Painting EASY](<https://www.youtube.com/watch?v=4VI5jk7pZag>)

**열람:** 원본 자동 영어 자막 발췌 및 08:19–11:30 읽음

**사용 범위와 한계:** Surface Deform 시험과 실제 웨이트 전송 차이. 기본 계획에서는 보조 표면 방식 제외.

<a id="source-31"></a>

## 31. Importing skinned Meshes

Unity Technologies · 2019.1, 역사적 공식 문서 · 열람 2026-09-09

[Importing skinned Meshes](<https://docs.unity.cn/2019.1/Documentation/Manual/ImportingSkinnedMeshes.html>)

**열람:** 공식 본문 확인

**사용 범위와 한계:** Unity의 선형 스키닝 설명에 한정. 현재 Import UI·허용 최대 영향 수는 2022.3 문서와 구분.

<a id="source-32"></a>

## 32. FBX exporter — fbx_data_mesh_shapes_elements

Blender 설치 코드 · 설치 Blender 5.2.1 LTS · 열람 2026-09-09

FBX exporter — fbx_data_mesh_shapes_elements (로컬 설치 코드 확인; 설치 경로 생략)

**열람:** 로컬 소스 함수/BlendShape 쓰기 코드 읽음

**사용 범위와 한계:** 코드 존재는 기능 경로의 증거. 현재 캐릭터의 실제 FBX 왕복 성공을 뜻하지 않음.

<a id="source-33"></a>

## 33. v006 Model Inventory

프로젝트 읽기 전용 검사 · 2026-09-09 · 열람 2026-09-09

v006 Model Inventory (공유본 미포함: `MODEL_INVENTORY.json`)

**열람:** Blender CLI 로드·본/부품/키/모디파이어 검사·중립 렌더·SHA 불변 확인

**사용 범위와 한계:** 주 손 성분과 예전 component_map 범위를 검사. 새 무릎 정점을 과거 맵에 무조건 대응시키지 않음.

<a id="source-34"></a>

## 34. v007 관절 재검수

프로젝트 재검수 · 2026-09-09 · 열람 2026-09-09

[v007 관절 재검수](../v007_joint_audit/REVIEW.md)

**열람:** 실제 렌더·임시 리그 생성 규칙·검증 한계 확인

**사용 범위와 한계:** 사용자 피드백 반영. 수정 완료 자료가 아님.

## 교차 확인에서 해결한 차이

- 오래된 SDK2 손가락/UpperChest 제한을 SDK3에 그대로 적용하지 않았다.
- FBX Legacy 문서의 shape-key 제한은 설치 exporter 코드와 충돌하므로 실제 왕복 검증을 결론으로 삼았다.
- Blender 강의의 DQ·Corrective Smooth·Bendy Bones 결과를 표준 PC 런타임 결과로 취급하지 않았다.
- 자동 자막의 손가락 옆 굽힘 설명은 MCP와 마디 관절을 나눠 해석했다.
- Bone Roll 정리 조언은 모든 본의 값을 0으로 만드는 규칙으로 옮기지 않았다.
- 현재 설치/시험 Unity 버전과 VRChat 공식 지원 버전을 구분했다.

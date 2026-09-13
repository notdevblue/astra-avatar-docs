> **기록 시점 안내:** 아래는 해당 단계 당시의 보고서입니다. 후속 결정은 [최신 상태](../../STATUS.md)를 기준으로 읽어주세요. 모델·원본 텍스처·검증 원시 데이터는 이 문서 공유본에 포함하지 않습니다.

# v352 — 전달 파일과 재현

선택된 검수본은 아래 두 파일이다. 최종 채택이나 PC VRChat 클라이언트 완료본은 아니다.

- Blender: `trials/bianca_v352_NPR_REVIEW.blend`. 활성 기본색8개 packed 포함, 원본 헤어8K 편집용 유지.
- Unity: 프로젝트 `unity_vrchat_avatar`의 `Assets/BiancaV352Review/Bianca_v352_Final.prefab`. 기존 프로젝트를 Unity2022.3.22f1에서 열어 확인한다. 해당 폴더만 떼면 기존 v351 재질·컨트롤러·SDK 의존성이 빠진다.
- `Bianca_v352_SurfaceCandidate.prefab` 및 EYETIP/HAIR/GUARD 이름의 파일은 중간 비교용이다. 선택 파일을 대신하지 않는다.
- 실제 최종 정적/연속/복귀 데이터는 `final_rig_v2`, 물리는 `final_physics`다. 첫 `final_rig`는 중립 초기화 실패 기록이다.

모델·PNG·Unity자산·코드·원시 검증은 작업 저장소에만 보관한다. 문서 저장소에는 이 문서와 선별 렌더/GIF/MP4만 들어간다. 파일 식별값은 작업본의 DELIVERY.json에 있다.

## 검증 자료 보관

수천 개 스킨 원시 파일은 `evidence`의45MB 이하 원본 배치별 tar.gz로 묶고 모든 구성 파일을 SHA256으로 다시 확인했다. 대형 동작 JSON도 gzip 원문 일치를 확인했다. 작업 루트에서 다음 명령으로 복원한다.

```sh
python3 avatar_modeling/v352_eye_shadow_research/scripts/restore_evidence.py
```

복원기는 목록에 있는 파일만 경로·해시를 확인하여 쓴다. 각 단계의 scripts와 실행 JSON에 입력·출력 조건이 있다. 이미지 생성 중간 채색 결과는 작업본 textures에 남겼으므로 재현에 새 이미지 생성 응답이 필수는 아니다.

중간 자동 .blend 백업, 영상 조립용 중복 프레임, 동일 raw의 비압축 중복은 Git에서 제외하고 로컬에 보존한다. 선택 Blender와 제작 코드·텍스처·실제 측정 원시 아카이브·사진·최종 영상은 포함한다. 원시 아카이브는 검증 근거이며 미채택 모델을 최종으로 덮어쓰는 도구가 아니다.

## 실행 순서와 유의점

1. 기존 v351·v348 의존성을 포함한 작업 저장소에서 실행한다. 현 사용자 파일을 덮어쓰지 않고 사본을 만든다.
2. 눈/코트/커프/바지 native UV 베이크 코드 → combine_final.py → Unity FinalSetup 순서다. 실제 채택 텍스처는 최종 packed PNG 일치 감사에 대응한다.
3. Unity GUI에서 최종 프리팹의 실제 스킨을 검사한다. 리그 검사 때 물리를 잠시 끄는 것과 전달 프리팹의11 PhysBone 활성 상태를 구분한다.
4. 최종 리그 V2는 기존 실제 muscle/body 입력을 명시한다. Rebind 직후 값을 중립으로 간주한 첫 실패 방식을 재사용하지 않는다.
5. 최종 물리는 준비90프레임 후80초 입력을 두 번 반복한다. Editor 파라미터 바람이며 실제 월드 감지가 아니다.

[검증 결과와 미해결](v352_WORK_REVIEW.md)

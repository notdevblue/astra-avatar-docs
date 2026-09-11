# Mac용 VRChat SDK 프로젝트 준비

2026-09-11, Creator Companion이 없는 Mac에서 별도 아바타 프로젝트를 준비하고 첫 실행을 확인했다. 기존 아바타 모델의 채택 상태나 수정 범위는 바뀌지 않았다.

- 공식 Avatar 템플릿을 사용하고 Unity 2022.3.22f1 Apple Silicon 및 Windows Build Support Mono를 설치했다.
- 공식 배포본 SHA-256을 대조해 VRChat Base/Avatars 3.10.5와 VPM Resolver 0.1.29를 설치했다.
- 사용자 Hub 실행 후 라이선스 인증, 초기 패키지 가져오기와 SDK 컴파일 완료를 확인했다. C# 컴파일 오류는 0개였다.
- 실행 중 설정은 StandaloneWindows64, Linear였다. SDK 환영 창과 Authentication 제어판을 실제 확인했다.

초기 터미널 실행의 라이선스 오류는 Hub 실행 후 해소됐다. Apple Silicon에서는 SDK의 OculusSpatializer 플러그인이 x86_64/i386 바이너리여서 로드 경고가 남는다. SDK 제어판은 정상적으로 열렸으며 공간음향 미리보기·아바타 빌드·업로드·클라이언트 실행은 이번 검증 범위에 포함하지 않는다.

기존 모델을 가져오거나 변경하지 않은 빈 제작 프로젝트다. 프로젝트 원본·SDK·실행 코드·로그는 이 문서 저장소에 포함하지 않는다.

## 공식 근거

2026-09-11 확인: [공식 Avatar 템플릿](https://github.com/vrchat-community/template-avatar), [Creator Companion 없이 템플릿 사용](https://vcc.docs.vrchat.com/guides/using-project-template-repos/), [지원 Unity 버전](https://creators.vrchat.com/sdk/upgrade/current-unity-version/).

---
publish: true
created: 2025-05-16T02:32:29.449+09:00
modified: 2026-04-02T20:03:04.379+09:00
---

## References

- Flutter [doc사이트](https://docs.flutter.dev/get-started/install)
- Chocolatery [커뮤니티 사이트](https://community.chocolatey.org/)
- Android Studio [사이트](https://developer.android.com/studio?hl=ko)
- Visual Studio [사이트](https://visualstudio.microsoft.com/ko/downloads/)

## Flutter 설치 순서

1. Flutter 사이트를 통해서 기본적인 정보를 확인한다. 하지만 다운로드와 Path 설정은 하지 않음.
2. Chocolatery 사이트로 가서 매뉴얼 참고하여 Chocolatery 개인(**Individual**) 버전을 설치
   이미 설치가 되어 있다면 아래를 실행하여 업그레이드 진행

```powershell
choco upgrade chocolatey
```

3. 아래의 Flutter 설치 명령어 실행

```powershell
choco install flutter
```

4. Flutter doctor 수행하여 제대로 설치되었는지 확인한다.

```PowerShell
Doctor summary (to see all details, run flutter doctor -v):
[√] Flutter (Channel stable, 3.29.0, on Microsoft Windows [Version 10.0.19045.5608], locale ko-KR)
[√] Windows Version (10 Home 64비트, 22H2, 2009)
[!] Android toolchain - develop for Android devices (Android SDK version 35.0.1)
    X cmdline-tools component is missing
      Run `path/to/sdkmanager --install "cmdline-tools;latest"`
      See https://developer.android.com/studio/command-line for more details.
    X Android license status unknown.
      Run `flutter doctor --android-licenses` to accept the SDK licenses.
      See https://flutter.dev/to/windows-android-setup for more details.
[√] Chrome - develop for the web
[X] Visual Studio - develop Windows apps
    X Visual Studio not installed; this is necessary to develop Windows apps.
      Download at https://visualstudio.microsoft.com/downloads/.
      Please install the "Desktop development with C++" workload, including all of its default components
[√] Android Studio (version 2024.3)
[√] VS Code (version 1.97.2)
[√] Connected device (3 available)
[√] Network resources

! Doctor found issues in 2 categories.
```

4. Android Studio 설치한 뒤 Setting에서 skdmanager를 검색하여 cmdline-tools를 설치한다.
5. 다시 Flutter doctor 수행

```PowerShell
Doctor summary (to see all details, run flutter doctor -v):
[√] Flutter (Channel stable, 3.29.0, on Microsoft Windows [Version 10.0.19045.5608], locale ko-KR)
[√] Windows Version (10 Home 64비트, 22H2, 2009)
[!] Android toolchain - develop for Android devices (Android SDK version 35.0.1)
    ! Some Android licenses not accepted. To resolve this, run: flutter doctor --android-licenses
[√] Chrome - develop for the web
[X] Visual Studio - develop Windows apps
    X Visual Studio not installed; this is necessary to develop Windows apps.
      Download at https://visualstudio.microsoft.com/downloads/.
      Please install the "Desktop development with C++" workload, including all of its default components
[√] Android Studio (version 2024.3)
[√] VS Code (version 1.97.2)
[√] Connected device (3 available)
[√] Network resources

! Doctor found issues in 2 categories.
```

6. flutter doctor 실행 시 --android-licenses 옵션을 추가하여 실행
7. 다시 Flutter doctor 수행

```PowerShell
Doctor summary (to see all details, run flutter doctor -v):
[√] Flutter (Channel stable, 3.29.0, on Microsoft Windows [Version 10.0.19045.5608], locale ko-KR)
[√] Windows Version (10 Home 64비트, 22H2, 2009)
[√] Android toolchain - develop for Android devices (Android SDK version 35.0.1)
[√] Chrome - develop for the web
[X] Visual Studio - develop Windows apps
    X Visual Studio not installed; this is necessary to develop Windows apps.
      Download at https://visualstudio.microsoft.com/downloads/.
      Please install the "Desktop development with C++" workload, including all of its default components
[√] Android Studio (version 2024.3)
[√] VS Code (version 1.97.2)
[√] Connected device (3 available)
[√] Network resources

! Doctor found issues in 1 category.
```

8. Visual Studio 커뮤니티를 다운받아 설치 후 Desktop관련 패키지를 설치한다.
9. 다시 Flutter Doctor 수행

```Powershell
PS C:\Windows\system32> flutter doctor
Doctor summary (to see all details, run flutter doctor -v):
[√] Flutter (Channel stable, 3.13.9, on Microsoft Windows [Version 10.0.19045.3693], locale ko-KR)
[√] Windows Version (Installed version of Windows is version 10 or higher)
[√] Android toolchain - develop for Android devices (Android SDK version 33.0.2)
[√] Chrome - develop for the web
[√] Visual Studio - develop Windows apps (Visual Studio Build Tools 2019 16.11.24)
[√] Android Studio (version 2022.3)
[√] VS Code, 64-bit edition (version 1.84.2)
[√] Connected device (3 available)
[√] Network resources
```

10. 다 해결됨, 만약 체크가 되지 않거나 한 부분은 구글 검색하여 해결 필요
11. Flutter 실행 후 결과 확인: 별 다른 에러가 없다면 설치 완료.
12. 아래와 같이 Terminal에서 명령어를 입력하여 Project 생성한다.

```Powershell
flutter create PJTNAME
```

## Visual Studio Code 설정

### Flutter 실행 에러

위에서 Flutter설치 후 프로젝트를 생성하면 정상적으로 진행된다.
그러나 VS에서 Flutter 프로젝트를 생성하려고 하면 아래와 같은 에러가 발생한다.

```Powershell
Unable to find git in your PATH
```

해결방법 : [참고링크](https://github.com/flutter/flutter/issues/123995)

Powershell 관리자 모드에서 아래 명령어 입력(git config 설정)

```Powershell
git config --global --add safe.directory '*'
```

### Extension 설치

Dart, Flutter, Error Lens 설치하기

### Setting.json 설정

```json
"editor.codeActionsOnSave": {
	"source.fixAll": "explicit"
},
"dart.previewFlutterUiGuides": true,
```

## Android Studio

### Flutter 실행 에러 1

```Powershell
PS C:\Windows\system32> flutter doctor
871f65ac1bf129edb222c3293a636ff4b67534a6은(는) 예상되지 않았습니다.
```

```
PS C:\Windows\system32> choco uninstall flutter
PS C:\Windows\system32> choco install flutter
```

저 **~은(는) 예상되지 않았습니다.** 메세지는 결국 해결하지 못했고
flutter 삭제하고 다시 설치하는 방법밖에 없었다.

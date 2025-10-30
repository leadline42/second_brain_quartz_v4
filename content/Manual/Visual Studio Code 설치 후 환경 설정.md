
일반적으로 설치 후 Welcome Page를 단계별로 따라가도 된다.
그러나 별도의 설정과 관련하여 아래와 같이 정리한다.
# Extension 설치


# Code 설치와 적용

1. Python 설치

Python 설치 후 가상 환경 생성 시 활성화가 안될 때 발생하는 오류
```powershell
PS D:\Programming\Python> python -m venv ./venv_py3124_RPA
PS D:\Programming\Python> .\venv_py3124_RPA\Scripts\activate
.\venv_py3124_RPA\Scripts\activate : 이 시스템에서 스크립트를 실행할 수 없으므로 D:\Programming\Python\venv_py3124_RPA\Scripts\Activate.ps1 파일을 
 로드할 수 없습니다. 자세한 내용은 about_Execution_Policies(https://go.microsoft.com/fwlink/?LinkID=135170)를 참조하십시오.
위치 줄:1 문자:1
+ .\venv_py3124_RPA\Scripts\activate
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : 보안 오류: (:) [], PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess
```


- `Powershell`은 Script 파일을 실행할 수 없도록 기본 설정되어 있다.
- 설정 가능한 값
    - `Restricted`: default, Script 파일 실행 불가
    - `AllSigned`: 서명된(승인된) Script 파일만 실행
    - `RemoteSigned`: 현 시스템에서 사용자가 생성한 Script와 서명된(승인된) Script 파일만 실행
    - `Unrestricted`: 모든 Script 파일 실행 가능
    - `ByPass`: 경고 및 차단 없이 모든 Script 파일 실행
    - `Undefined`: 권한 설정 안함

권한 확인 후 재설정
검색창 → `Powershell` → 관리자 권한 실행 → Script 실행 권한 변경

```Powershell
# 현재 권한 확인
get-ExecutionPolicy

# 권한 설정(RemoteSigned)
Set-ExecutionPolicy RemoteSigned

# ex)
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

새로운 크로스 플랫폼 PowerShell 사용 https://aka.ms/pscore6

PS C:\Windows\system32> get-ExecutionPolicy
Restricted
PS C:\Windows\system32> Set-ExecutionPolicy RemoteSigned

실행 규칙 변경
실행 정책은 신뢰하지 않는 스크립트로부터 사용자를 보호합니다. 실행 정책을 변경하면 about_Execution_Policies 도움말
항목(https://go.microsoft.com/fwlink/?LinkID=135170)에 설명된 보안 위험에 노출될 수 있습니다. 실행 정책을
변경하시겠습니까?
[Y] 예(Y)  [A] 모두 예(A)  [N] 아니요(N)  [L] 모두 아니요(L)  [S] 일시 중단(S)  [?] 도움말 (기본값은 "N"): y
PS C:\Windows\system32> get-ExecutionPolicy
RemoteSigned
```

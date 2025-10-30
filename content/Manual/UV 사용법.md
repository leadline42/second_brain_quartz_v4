Python Package 관리 툴인 UV 설치와 사용법을 정리하였다.
Rust도 함께 배우는 입장이라 익숙한 느낌이 있었고 속도가 엄청 빠르다!
# Reference

Git Site: [https://github.com/astral-sh/uv](https://github.com/astral-sh/uv)
Document: [https://docs.astral.sh/uv/](https://docs.astral.sh/uv/)
# uv 설치 (standalone)

## Windows

```powershell
# On Windows.
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```
## macOS and Linux

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

# uv에서 Package 설치

Project 관리와 Tools 설치, 그리고 Tools로 지원하지 않는 부분에 대해서는 pip로 진행한다.
Project 생성, 의존성 추가/제거, 실행 관리 등에 대한 기능을 지원한다.

기본적으로 pip 로 설치할  package에 대해서는 add를 이용해 package를 설치한다. 

## 개별 package 설치 시 ex) Panda

```powershell
uv add pandas --allow-insecure-host pypi.org --allow-insecure-host files.pythonhosted.org
```
**--allow-insecure-host 옵션은 인증이 제한된 사내 등에서 활용한다.**

### vscode 등 개발을 위한 개발용 인터랙티브 package 추가 시 --dev옵션을 사용

```powershell
uv add --dev ipykernel --allow-insecure-host pypi.org --allow-insecure-host files.pythonhosted.org # 개발용(인터랙티브) 의존성 추가
```
### 제공된 requirements.txt 파일 활용 시 

```powershell
uv add -r requirements.txt  # 주요 의존성 설치
``` 
### pytorch 경우 설치 옵션이 좀 복잡해서 아래와 같이 기존의 pip interface 그대로 활용한다.
```powershell
uv pip install --trusted-host pypi.org --trusted-host files.pythonhosted.org --trusted-host https://download.pytorch.org/whl/cu126 torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu126
```


# Python versions 확인과 설치

기본 Python이 설치되어 있는 상태에서 초기회(uv init 폴더명)을 하게 되면 기본 설치된 Python을 활용한 가상환경이 설정된다. 여기에 추가로 다른 Python Version을 추가하기 위해서는 아래와 같은 과정을 진행하면 된다.

물론 초기화 시 Python Version 을 지정할 수도 있다.
```Powershell
uv init -p 3.12 folder_name
```

### Version 리스트 확인

```Powershell
uv python list
```
### Python  version을 지정하여 설치 (사내망)

```Powershell
uv python install 3.12 --allow-insecure-host https://github.com/astral-sh
```
### 초기화 시 Python version 지정 
 
```Powershell
uv init -p 3.12 folder_name
```
### 가상환경 생성

3.12로만 사용해도 되고 기존 다른 python 버전에 추가 시 아래와 같이 가상 환경도 추가해줘야 한다.

```Powershell
uv venv -p 3.12 .venv-312
```
### 프로젝트 클론 후 환경 재현 시 pyproject.toml과 uv.lock 기반으로 .venv 현재 프로젝트 설정으로 가상환경 동기화

```Powershell
uv sync
```

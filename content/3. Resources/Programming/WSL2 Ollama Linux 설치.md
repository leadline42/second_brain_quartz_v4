---
publish: true
created: 2025-10-24T00:22:47.922+09:00
modified: 2026-04-02T20:16:13.827+09:00
tags:
  - resource
---

Links:

# **사전 확인 사항**

## **1. WSL2 설정과 Ubuntu 설치 후 Systemd 사용을 위해 설정을 진행**

[WSL2 systemd 사용하기](https://junk-s.tistory.com/entry/WSL-WSL2%EC%97%90%EC%84%9C-Systemd-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)

1. pstree를 사용하여 확인

```bash
pstree

systemd─┬─2*[agetty]
        ├─containerd───13*[{containerd}]
        ├─containerd-shim─┬─weaviate───13*[{weaviate}]
        │                 └─11*[{containerd-shim}]
        ├─cron
        ├─dbus-daemon
        ├─dockerd─┬─2*[docker-proxy───7*[{docker-proxy}]]
        │         ├─docker-proxy───8*[{docker-proxy}]
        │         ├─docker-proxy───6*[{docker-proxy}]
        │         └─17*[{dockerd}]
        ├─init-systemd(Ub─┬─SessionLeader───Relay(773)───bash───pstree
        │                 ├─init───{init}
        │                 ├─login───bash
        │                 └─{init-systemd(Ub}
        ├─ollama───10*[{ollama}]
        ├─rsyslogd───3*[{rsyslogd}]
        ├─systemd───(sd-pam)
        ├─systemd-journal
        ├─systemd-logind
        ├─systemd-resolve
        ├─systemd-timesyn───{systemd-timesyn}
        ├─systemd-udevd
        ├─unattended-upgr───{unattended-upgr}
        └─wsl-pro-service───7*[{wsl-pro-service}]
```

기본적으로 WSL을 사용하면 init을 사용하나 최신 WSL는 systemd를 사용
확인 후 설정 진행

# WSL2 ollama Linux 버전 설치

## 1. 아래 안내 사항에 따라 설치

<https://github.com/ollama/ollama/blob/main/docs/linux.md>

## 2. Manual Install

만약 회사 같은 내부망에서 WSL2를 사용하는 경우는 경우 Manual install를 참고해서 진행한다.

그리고 아래와 같이 curl 사용 시 SSL오류 발생하는데

```bash
curl: (60) SSL certificate problem: self-signed certificate in certificate chain
```

이럴 경우 아래와 같이 -k 옵션을 이용해서 다운로드 진행한다.

```bash
curl -k -LO https://ollama.com/download/ollama-linux-amd64.tgz
```

상세한 내용은 [TLS Certificate Verification](https://curl.se/docs/sslcerts.html)을 참고

```bash
sudo rm -rf /usr/lib/ollama
sudo tar -C /usr -xzf ollama-linux-amd64.tgz
```

## 3. Startup service 관련 추가 사항

ollama.service 파일 설정은 아래와 같다.

```bash
[Unit]
Description=Ollama Service
After=network-online.target
[Service]
ExecStart=/usr/bin/ollama serve
User=ollama
Group=ollama
Restart=always
RestartSec=3
Environment="PATH=$PATH:OLLAMA_HOST=0.0.0.0:11434"

[Install]
WantedBy=multi-user.target
```

외부 접근을 위해서는 
Environment="PATH=$PATH" 에 "OLLAMA\_HOST=0.0.0.0:11434"를 추가해준다.

# WSL2에 CUDA설치 

## 1. 아래 다운로드 링크의 안내 사항에 따라 설치

<https://developer.nvidia.com/cuda-12-6-3-download-archive?target_os=Linux&target_arch=x86_64&Distribution=WSL-Ubuntu&target_version=2.0&target_type=deb_network>

만약 회사 등 내부망에서 wget 사용 시 certification 에러 발생 시 --no-check-certificate 옵션 적용

```bash
wget --no-check-certificate https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-keyring_1.1-1_all.deb
```

```bash
sudo dpkg -i cuda-keyring_1.1-1_all.deb
sudo apt-get update
```

그러나 역시나 내부망에서 update를 진행하면 update중에 오류가 발생한다.
이럴 경우 /etc/apt/apt.conf.d/ 아래의 99verify-peer.conf 파일을 생성한 뒤
Acquire { https::Verify-Peer false } 를 추가한다.
상세내용은 [Link\_Ubuntu\_Q\&A](https://askubuntu.com/a/1210812)를 참고한다.

```bash
sudo apt-get -y install cuda-toolkit-12-6
```

# WSL2에 cuDNN설치

## 1. 아래 다운로드 링크의 안내 사항에 따라 설치

<https://developer.nvidia.com/cudnn-downloads?target_os=Linux&target_arch=x86_64&Distribution=Ubuntu&target_version=24.04&target_type=deb_network&Configuration=Full>

CUDA version에 맞추어서 진행하는데 Intstaller Type이 network 진행 시,
아래와 같이 오류가 발생할 수 있다. 이런 상황에서는 local  다운로드 후 진행하면 된다.

```bash
~$ sudo apt-get -y install cudnn9-cuda-12
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
E: Unable to locate package cudnn9-cuda-12
```

# 추가 문제 해결

## 1. Ollama 모델 다운로드는 WSL에 Pull이 SSL오류로 진행이 안될 경우

Desktop용으로 설치 후 모델을 다운로드하여 WSL2로 복사
Windows 경로: C:\Users\jongwan.choi.ollama
WSL2 복사 경로 : /usr/share/ollama/.ollama/

## 2. WSL2의 Ollama를 윈도우에서 사용

### 1) 포트 포워딩

Reference: [Windiws WSL2 포트포워딩 방법](https://velog.io/@ung6860/%EA%B0%9C%EB%B0%9C%ED%99%98%EA%B2%BDWSL-%EC%99%B8%EB%B6%80-%EC%A0%91%EC%86%8D%ED%95%98%EA%B8%B0#wsl%EC%9D%98-%EC%99%B8%EB%B6%80%EC%A0%91%EC%86%8D-%ED%9D%90%EB%A6%84)

WSL2에 Ollama설치 완료 후 Ollama를 윈도우(ex vscode)에서 사용하기 위해서는 포트포워딩을 통해서 포트를 오픈을 해주어야 함

### 2) WSL2에 net-tools 설치

```bash
sudo apt-get install net-tools
```

### 3) WSL2 내 아이피 확인

```bash
ifconfig 
```

### 4) Windows 작업

포트 포워딩 스크립트는 윈도우에서 실행하므로 ports\_wsl.ps1 확장자로 작성
내용은 아래 참고

```powershell
$remoteport = bash.exe -c "ifconfig eth0 | grep 'inet '"
$found = $remoteport -match '\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}';

if( $found ){
  $remoteport = $matches[0];
} else{
  echo "The Script Exited, the ip address of WSL 2 cannot be found";
  exit;
}

# 해당 위치에 포워딩을 희망하는 포트를 나열해준다.
# $ports=@(80,443,5000,22);
# ollama port는 11434
$ports=@(11434);

$addr='0.0.0.0';
$ports_a = $ports -join ",";

iex "Remove-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock' ";

iex "New-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock' -Direction Outbound -LocalPort $ports_a -Action Allow -Protocol TCP";
iex "New-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock' -Direction Inbound -LocalPort $ports_a -Action Allow -Protocol TCP";

for( $i = 0; $i -lt $ports.length; $i++ ){
  $port = $ports[$i];
  iex "netsh interface portproxy delete v4tov4 listenport=$port listenaddress=$addr";
  iex "netsh interface portproxy add v4tov4 listenport=$port listenaddress=$addr connectport=$port connectaddress=$remoteport";
}
Invoke-Expression "netsh interface portproxy show v4tov4";
```

그리고 Powershell에서 작성된 스크립트 실행 .\ports\_wsl.ps1
실행 시 처음에는 아래와 같은 에러가 발생할 수 있으나 무시하면 됨
(WSL 2 Firewall Unlock은 처음에는 없기 때문에 삭제를 할 수 없어 발생하는 에러)

```powershell
Remove-NetFireWallRule : 'WSL 2 Firewall Unlock'과(와) 같은 'DisplayName' 속성을 가진 MSFT_NetFirewallRule 개체가 없습니다. 속성 값을 검증하고 다시 시도하십시오.
위치 줄:1 문자:1
+ Remove-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock'
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : ObjectNotFound: (WSL 2 Firewall Unlock:String) [Remove-NetFirewallRule], CimJobException
    + FullyQualifiedErrorId : CmdletizationQuery_NotFound_DisplayName,Remove-NetFirewallRule
```

이제 wsl의 설정을 완료했으니 Windows 머신의 포트 액세스를 허용해주자.
윈도우에서 인바운드 규칙을 설정한다.

Windows+R > firewall.cpl 입력 후 실행
Windows Defender 방화벽 → 고급 설정 → 인바운드 규칙 → 새규칙
포트 선택 → TCP 선택 → 특정 로컬 포트 선택 → 포트 입력
연결 허용 → 다음 → 프로필 → 다음 → 이름 부여

마지막으로 wsl 포트 포워딩 설정 (윈도우에서 실행, WSL2\_IP는 위에서 ifocnfig를 통해서 확인 가능)

```powershell
netsh interface portproxy add v4tov4 listenport=11434 listenaddress=0.0.0.0 connectport=11434 connectaddress=[WSL2_IP]
```

tcp(Ipv4)로 구동되어야하는 프로세스가 tcp6(Ipv6)로 구동된다.
이 때 Ipv6 미사용 설정하면 된다.

```bash
# 입력
sudo sysctl -w net.ipv6.conf.all.disable_ipv6=1
# 결과 net.ipv6.conf.all.disable_ipv6 = 1

# 입력
sudo sysctl -a | grep net.ipv6.conf | grep disable_ipv6
# 결과
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.eth0.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
```

실제 외부 이용 시 접속 주소는 WSL2 IP:11434가 아니라 PC IP:11434로 사용해야한다.
포트포워딩이 때문에~ 내부(Local PC)에서 접속할 때는 WSL2 IP를 사용해도 된다.

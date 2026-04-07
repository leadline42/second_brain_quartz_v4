---
publish: true
created: 2026-04-03T02:04:21.313+09:00
modified: 2026-04-07T22:59:21.074+09:00
tags:
  - resource
---

Links:

WSL에서 USB장치를 연결하는 것은 오픈소스 프로젝트인 usbipd-win이라는 프로젝트로 가능하다.

# References

- [Blog](https://lucidmaj7.tistory.com/388)
- [MS Build 2026](https://learn.microsoft.com/ko-kr/windows/wsl/connect-usb)
- [USBIPD](https://github.com/dorssel/usbipd-win/releases)
- [WSL2 안드로이드 개발 환경 구성](https://jsonobject.tistory.com/635#google_vignette)

weaviate Site: https://docs.weaviate.io/deploy/installation-guides/docker-installation

## 설치 환경

- WSL2 (Windows 10)
- x64/x86 CPU
- WSL2에 설치된 Linux(커널 5.10.60.1 이상)

## 1. USBIPD-WIN 설치

```Powershell
winget install --interactive --exact dorssel.usbipd-win
```

## 2. USBIPD 실행

```Powershell
ubsipd

usbipd-win 5.3.0

Description:
  Shares locally connected USB devices to other machines, including Hyper-V guests and WSL 2.

Usage:
  usbipd [command] [options]

Options:
  -?, -h, --help  Show help and usage information
  --version       Show version information

Commands:
  attach   Attach a USB device to a client
  bind     Bind device
  detach   Detach a USB device from a client
  license  Display license information
  list     List USB devices
  policy   Manage policy rules
  server   Run the server on the console
  state    Output state in JSON
  unbind   Unbind device

```

## 3. WSL2 설정

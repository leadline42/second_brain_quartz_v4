---
publish: true
created: 2025-05-21T03:23:57.923+09:00
modified: 2026-05-09T03:32:32.336+09:00
tags:
  - resource
---

Links:

# References

- [Guide Page](https://dev.to/koolkamalkishor/running-llama-32-on-android-a-step-by-step-guide-using-ollama-54ig)

## Termux 설치 (Install Termux on Android)

[Releases · termux/termux-app](https://github.com/termux/termux-app/releases) 에서 App을 다운로드 받아서 설치한다.

## Termux 설정 (Set Up Termux)

Grant Storage Access:

```linux
termux-setup-storage
```

This command lets Termux access your Android device’s storage, enabling easier file management.

Update Packages:

```linux
pkg upgrade
```

Enter Y when prompted to update Termux and all installed packages.

Install Essential Tools:

```linux
pkg install git cmake golang
```

These packages include Git for version control, CMake for building software, and Go, the programming language in which Ollama is written.

## Ollama 설치와 컴파일하기 (Install and Compile Ollama)

[ollama/docs/development.md at main · ollama/ollama · GitHub](https://github.com/ollama/ollama/blob/main/docs/development.md)
Ollama is a platform for running large models locally. Here’s how to install and set it up:

Clone Ollama's GitHub Repository:

```linux
git clone --depth 1 https://github.com/ollama/ollama.git
```

Navigate to the Ollama Directory:

```linux
cd ollama
```

Generate Go Code:

```linux
go generate ./...
```

Build Ollama:

```linux
go build .
```

Start Ollama Server:

```linux
./ollama serve &
```

Now the Ollama server will run in the background, allowing you to interact with the models.

## Llama 3.2 모델 실행 (Running Llama 3.2 Models)

To run the Llama 3.2 model on your Android device, follow these steps:

Choose a Model:
Models like llama3.2:3b (3 billion parameters) are available for testing. These models are quantized for efficiency. You can find a list of available models on Ollama’s website.

Download and Run the Llama 3.2 Model:

```linux
./ollama run llama3.2:3b --verbose
```

The --verbose flag is optional and provides detailed logs. After the download is complete, you can start interacting with the model.

## Optional Cleanup

After using Ollama, you may want to clean up the system:

Remove Unnecessary Files:

```linux
chmod -R 700 ~/go
rm -r ~/go
```

Move the Ollama Binary to a Global Path:

```linux
cp ollama/ollama /data/data/com.termux/files/usr/bin/
```

Now, you can run ollama directly from the terminal.

잘 쓰지 않다 보니 까먹기 시작했다 정리해둬야지

# References

Docker Site: https://docs.docker.com/engine/install/ubuntu/

# WSL2 ubuntu Docker 설치하기

환경은 wsl2 ubuntu 24.04 버전이며 root 계정이면 그냥 진행 아니면 sudo 사용

## **1. apt 패키지 업데이트**

```bash
sudo apt-get update -y
sudo apt-get upgrade -y
```

## **2. Docker 설치 전 필수 패키지 설치**

```bash
sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common
```

## **3. Docker GPG 키 & 저장소 추가**

```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

## **4. Docker 설치**

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## **5. User 계정에 Docker 권한 추가**

wsl은 기본 root 계정이 아니라 사용자 계정이다. 
따라서 일반 계정에서 docker를 사용하려면 docker group 권한을 추가해야 한다.

```bash
sudo usermod -aG docker $USER
```

## **6. Docker 서비스 실행**

```bash
sudo service docker start
```

## **7. 현재 session 로그아웃하고 재연결하기 (wsl이라서 그냥 껐다 켜면 된다.)**

## **8. Docker 실행 테스트 (User 계정)**

```bash
docker run hello-world
docker images
```
# 내 컴퓨터에 개발 환경 구축하기

## 1. 프로젝트 개요

이 프로젝트는 코드가 "내 컴퓨터에서만" 돌아가는 문제를 줄이고, 팀원 누구나 같은 방식으로 실행, 배포, 디버깅할 수 있는 환경 구성을 목표로 핵심 도구인 리눅스 CLI, Docker, Git/GitHub를 실습해본다.

## 2. 실행 환경

- OS: Windows 11
- Shell: bash
- Docker 버전: [실제 버전 작성]
- Git 버전: [실제 버전 작성]

## 3. 수행 체크리스트

- [ ] 터미널 기본 조작 및 폴더 구성
- [ ] 파일 및 디렉터리 권한 실습
- [ ] Docker 설치 및 동작 점검
- [ ] Docker 기본 운영 명령 수행
- [ ] 컨테이너 실행 실습
- [ ] Dockerfile 작성 및 커스텀 이미지 빌드
- [ ] 포트 매핑 및 웹 접속 확인
- [ ] Docker 볼륨 영속성 검증
- [ ] Git 설정 및 GitHub 연동
- [ ] 보안 및 개인정보 점검

## 4. 실습 수행

### 4.1 터미널 기본 조작 및 폴더 구성


### 4.2 파일 및 디렉터리 권한 실습


### 4.3 Docker 설치 및 기본 점검

Docker 버전 확인 실행

**실행 명령어**

```shell
docker --version
```

**실행 결과**

```text
Docker version 29.9.2, build a82e528
```

**캡처**
<img width="702" height="84" alt="Image" src="https://github.com/user-attachments/assets/04c9243a-c612-4956-b07b-422b40614ecf" />

Docker 동작 확인 실행

**실행 명령어**
```shell
docker info
```

**실행 결과**
<img width="1457" height="1607" alt="info" src="https://github.com/user-attachments/assets/4ef5f406-dc66-4401-abbe-27566c690b77" />

### 4.4 Docker 기본 운영 명령 수행

이미지 목록 확인


**실행 명령어**
```shell
docker images
```

**실행 결과**
<img width="1514" height="182" alt="docker img" src="https://github.com/user-attachments/assets/1bd5db71-36ff-4a0b-b514-b3f5c1e0ef02" />

컨테이너 목록 확인

**실행 명령어**
```shell
docker ps
```

**실행 결과**
<img width="1671" height="114" alt="image" src="https://github.com/user-attachments/assets/ba5d6743-abab-4e7c-8035-4d869b44b934" />

로그 확인

**실행 명령어**
```shell
docker logs [컨테이너 이름]
```

**실행 결과**
<img width="1529" height="1514" alt="logs" src="https://github.com/user-attachments/assets/dade8943-3cd5-4912-84bb-5e33960a591d" />


### 4.5 컨테이너 실행 실습

**hello-world 실행 결과**
<img width="1547" height="1047" alt="helloworld" src="https://github.com/user-attachments/assets/0b48acc5-daeb-418b-bac4-bec2d32a605d" />

**ubuntu 컨테이너 실행**
```shell
docker run -it --name [컨테이너 이름] ubuntu
ls
echo "hello from container"
cat /etc/os-release
```

**ubuntu 컨테이너 실행 결과**
<img width="1389" height="1116" alt="container" src="https://github.com/user-attachments/assets/bad9305e-641e-4a78-b97a-07e8132b4a06" />

**컨테이너 종료/유지의 차이 정리**

exit로 나오면 컨테이너가 멈춘 것을 확인할 수 있다.
<img width="2165" height="146" alt="exit" src="https://github.com/user-attachments/assets/403ceb82-8afc-4254-afaf-c7b1e59fd18c" />

반면 exec를 사용해서 들어갔다 나오면 여전히 컨테이너가 동작하는걸 확인할 수 있다.
<img width="2081" height="520" alt="exec" src="https://github.com/user-attachments/assets/446504d5-27ef-4fa8-ac68-3436c7f83887" />

컨테이너 중단 위험을 피하기 위해선 exec를 사용해서 터미널에 진입하는 것이 좋다.

### 4.6 Dockerfile 작성 및 커스텀 이미지 빌드

웹 서버 베이스 이미지(nginx)를 활용한 커스텀 이미지 생성
선택의 이유는 내가 만든 HTML 웹 페이지를 가볍고 쉽게 띄우기에 Nginx가 가장 적합해서 선택함.

```html
<!-- index.html -->
<!DOCTYPE html>
<html>
<head><meta charset="utf-8"><title>My First Container</title></head>
<body>
  <h1>안녕하세요, 홍길동입니다</h1>
  <p>이 페이지는 Docker 컨테이너 안에서 실행되고 있습니다.</p>
</body>
</html>
```

```docker
#Dockerfile
FROM nginx (베이스 이미지 정의)
COPY index.html /usr/share/nginx/html/index.html (nginx의 기본 index.html을 내가 만든 index.html로 덮어 씌움)
```

**index.html과 Dockerfile을 사용한 nginx를 베이스로 한 커스텀 이미지 제작**

```shell
# 커스텀 이미지 제작
docker build -t my-web . (.은 현재 폴더의 Dockerfile을 사용해서 이미지를 제작)

# 포트를 연결해서 컨테이너 실행
docker run -d -p 8080:80 --name my-web my-web (내 컴퓨터 8080번 포트 -> 컨테이너 80번 포트)

# Docker 상태 확인
docker ps
```

**이미지 빌드 성공 및 컨테이너 실행 성공**
<img width="904" height="1157" alt="docker image" src="https://github.com/user-attachments/assets/f8da8198-3899-4319-a383-0208e162a384" />
<img width="2084" height="205" alt="docker run" src="https://github.com/user-attachments/assets/fdafa461-4fb8-4ea0-a442-9f2561668c18" />

### 4.7 포트 매핑 및 웹 접속 확인

**정상적으로 작동하는 것을 확인**
<img width="782" height="496" alt="image" src="https://github.com/user-attachments/assets/c010fc00-6e59-401b-87d4-f976f34f1d99" />

### 4.8 Docker 볼륨 영속성 검증


### 4.9 Git 설정 및 GitHub 연동












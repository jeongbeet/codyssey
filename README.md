# 내 컴퓨터에 개발 환경 구축하기

## 1. 프로젝트 개요

이 프로젝트는 코드가 "내 컴퓨터에서만" 돌아가는 문제를 줄이고, 팀원 누구나 같은 방식으로 실행, 배포, 디버깅할 수 있는 환경 구성을 목표로 핵심 도구인 리눅스 CLI, Docker, Git/GitHub를 실습해본다.

## 2. 실행 환경

- OS: Windows 11, Ubuntu 26.04
- Shell: powershell, bash
- Docker 버전: 29.9.2
- Git 버전: 2.56.0

## 3. 수행 체크리스트

- [x] 터미널 기본 조작 및 폴더 구성
- [x] 파일 및 디렉터리 권한 실습
- [x] Docker 설치 및 동작 점검
- [x] Docker 기본 운영 명령 수행
- [x] 컨테이너 실행 실습
- [x] Dockerfile 작성 및 커스텀 이미지 빌드
- [x] 포트 매핑 및 웹 접속 확인
- [x] Docker 볼륨 영속성 검증
- [x] Git 설정 및 GitHub 연동
- [x] 보안 및 개인정보 점검

## 4. 실습 수행

### 4.1 터미널 기본 조작 및 폴더 구성

**현재 위치 확인과 목록 확인**
```bash
# 현재 위치 확인
pwd

# 목록 확인 (숨김 파일 포함)
ls -la
```

**실행 결과**
<img width="1183" height="997" alt="image" src="https://github.com/user-attachments/assets/803dcbd5-a798-4bbc-b501-f66f608dd213" />

**이동과 생성**

```bash
# test라는 디렉터리를 생성
mkdir test
# 생성한 test 디렉터리로 이동
cd test
# 현재 위치 확인
pwd
```

**실행 결과**

<img width="611" height="140" alt="image" src="https://github.com/user-attachments/assets/97482b53-673d-4691-b44e-1bee1fedbc13" />

**파일 생성, 복사, 이동/이름변경, 삭제**

```bash
# 빈 파일 생성
touch file.txt
# Hello를 파일에 저장
echo "Hello" > file.txt
# 파일 복사
cp file.txt copy.txt
# 파일 이동
mv copy.txt mydir/
# 파일 이름변경
mv file.txt new.txt
# 파일 삭제
rm new.txt
```

**실행 결과**
- file.txt와 그 복사본인 copy.txt가 만들어진 걸 확인할 수 있다.
<img width="919" height="184" alt="image" src="https://github.com/user-attachments/assets/69028afa-b217-41d8-8a05-390d41b32922" />

- copy.txt를 mydir로 이동하고 file.txt의 이름을 new.txt로 바꾼 후 삭제한 결과이다.
<img width="850" height="402" alt="image" src="https://github.com/user-attachments/assets/624b7d7d-40aa-4e36-8396-ae534eeca03d" />

**파일 내용 확인과 빈 파일 생성**

```bash
# 빈 파일 생성
touch empty.txt
# 내용 확인
cat empty.txt
# 빈 파일에 내용 넣기
echo "Hello" > empty.txt
# 내용 확인
cat empty.txt
```

**실행 결과**
- 빈 파일을 생성한 후 내용을 확인하니 아무것도 없는 게 확인된다. 빈 파일에 내용을 넣고 다시 확인해보면 Hello가 확인된다.
<img width="968" height="186" alt="image" src="https://github.com/user-attachments/assets/85cfc2c7-5f1b-4935-bbf2-24ef99781ab9" />



### 4.2 파일 및 디렉터리 권한 실습

**권한을 확인/변경하는 명령을 실습해보기 위해 file.txt와 mydir이라는 파일과 디렉토리를 생성**

```shell
# 컨테이너 안에서 실행
mkdir temp
cd temp

# file.txt 생성
touch file.txt

# mydir 디렉터리 생성
mkdir mydir

# 변경 전 권한 확인
ls -l
```

**변경 전 권한 확인 결과**
<img width="948" height="304" alt="권한 변경 전" src="https://github.com/user-attachments/assets/ee4b4bd4-fdb0-488f-8f43-3932b8116a2d" />

권한 변경 실행 

```shell
chmod 600 file.txt
chmod 700 mydir 

# 권한 변경 후 확인
ls -l
```

**변경 후 권한 확인 결과***
<img width="943" height="222" alt="권한 변경 후" src="https://github.com/user-attachments/assets/0162e5a8-3cd2-4c37-b079-eea8c1f7d267" />

**변경 전 후 권한 해석**
- 변경 전 file.txt의 권한 -rw-r--r-- / 변경 전: 소유자는 읽기·쓰기, 그룹과 기타 사용자는 읽기 가능
- 변경 후 file.txt의 권한 -rw------- / 변경 후: 소유자만 읽기·쓰기 가능

- 변경 전 mydir의 권한 drwxr-xr-x / 변경 전: 소유자는 모든 권한을 가지고, 그룹과 기타 사용자는 파일 목록을 보거나 디렉터리에 들어갈 수 있음
- 변경 후 mydir의 권한 drwx------ / 변경 후: 소유자만 목록 조회, 파일 생성·삭제, 디렉터리 접근 가능

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

**네트워크 네임스페이스와 포트포워딩 원리**

- 네트워크 네임스페이스는 네트워크 인터페이스, IP 주소, 라우팅 테이블, 포트 등의 네트워크 자원을 분리하는 Linux 기능이다. Docker의 기본 bridge 네트워크를 사용하는 Linux 컨테이너는 각각 독립된 네트워크 네임스페이스를 가진다. 따라서 여러 컨테이너가 내부에서 동일한 80번 포트를 사용해도 서로 충돌하지 않는다. 또한 컨테이너 내부의 localhost는 해당 컨테이너 자신을 의미하며, 호스트 컴퓨터의 localhost와 구분된다.

```powershell
docker run -d -p 8080:80 --name my-web my-web
```

- 여기서 '-p 8080:80'은 호스트의 8080번 포트를 컨테이너의 80번 포트에 연결한다는 의미이다. 브라우저에서 `http://localhost:8080`으로 접속하면 Docker가 요청을 컨테이너의 80번 포트로 전달하고, Nginx가 처리한 웹 페이지를 브라우저에 보여준다. 이러한 연결 전달이 포트포워딩이다.
- 컨테이너 내부 포트는 서로 같아도 되지만, 동일한 호스트 IP의 같은 TCP 포트를 여러 컨테이너에 동시에 게시하면 충돌한다. 따라서 다른 웹 컨테이너를 추가할 때는 `-p 8081:80`처럼 호스트 포트를 다르게 지정한다.

**정상적으로 작동하는 것을 확인**

<img width="782" height="496" alt="image" src="https://github.com/user-attachments/assets/c010fc00-6e59-401b-87d4-f976f34f1d99" />

### 4.8 Docker 볼륨 영속성 검증

**볼륨 만들기 및 볼륨을 연결한 컨테이너 실행**
```bash
docker volume create mydata
docker run -it --name vol-test -v mydata:/data ubuntu
```

**실행 결과**
<img width="1385" height="155" alt="볼륨" src="https://github.com/user-attachments/assets/b9f612c1-b1d8-4a13-862b-70e11022f3ac" />

**컨테이너 안에서 파일을 생성**
```bash
echo "hello volume" > /data/memo.txt (hello volume이라는 문장을 /data/memo.txt에 저장. /data에는 볼륨이 연결되어 있으므로, 이 파일은 mydata 볼륨에 저장됨.)
```

**실행 결과**
<img width="1091" height="36" alt="file" src="https://github.com/user-attachments/assets/784d32db-6b12-4bdb-b19c-576a52307b33" />

**컨테이너 삭제 후 새 컨테이너에서 파일 확인**
```bash
docker rm vol-test
docker run -it --rm -v mydata:/data ubuntu cat /data/memo.txt (기존 볼륨을 새 컨테이너의 /data에 연결 후 memo.txt의 파일 내용을 출력)
```

**실행 결과**

<img width="810" height="74" alt="컨테이너 삭제" src="https://github.com/user-attachments/assets/a6f6cf24-6a21-4a4c-8878-7595ac287a53" />

<img width="1394" height="108" alt="새 컨테이너 확인" src="https://github.com/user-attachments/assets/a23f6aa0-7bd9-4d05-9c84-da1fe44b80f2" />
정상적으로 hello volume이 출력


**볼륨 목록 확인**
```bash
docker volume ls
```


**실행 결과**

<img width="773" height="109" alt="볼륨 목록 확인" src="https://github.com/user-attachments/assets/2964b35a-d8b9-4a1d-9da4-e48f609594c6" />

데이터가 유지되고 mydata 볼륨이 존재하는 걸 확인하였으므로 볼륨 영속성을 검증함.

### 4.9 Git 설정 및 GitHub 연동

**Git 사용자 정보/기본 브랜치 설정**

```bash
# YOUR NAME과 EMAIL을 본인 것으로 수정
git config --global user.name "YOUR NAME"
git config --global user.email "YOUR EMAIL"

# 기본 브랜치 이름을 main으로 설정
git config --global init.defaultBranch main

# 설정 확인
git config --list
```

**설정 결과**

<img width="693" height="472" alt="git" src="https://github.com/user-attachments/assets/f4b6ee64-c79b-4aa1-beb6-8b71503c1e24" />

**GitHub 로그인 및 저장소 연동**

실습 파일을 보관할 폴더로 이동

```powershell
cd "C:\Users\사용자이름\Documents\실습폴더"
```

해당 폴더를 Git 저장소로 초기화

```powershell
git init
```

GitHub 저장소를 연결하고 확인

```powershell
git remote add origin https://github.com/내아이디/저장소이름.git
git remote -v
```

**실행 결과**

<img width="1916" height="265" alt="image" src="https://github.com/user-attachments/assets/a90bb3d2-c76c-4aee-8727-95cf66b00ea8" />

업로드할 파일을 선택 후 커밋

```powershell
git add . (.은 폴더 안에 있는 모든 파일을 포함)
git commit -m "원하는 말"
```

GitHub에 업로드하기

```powershell
git branch -M main
git push -u origin main
```

브라우저가 뜨면 로그인하고 인증

**실행 결과**
<img width="1196" height="520" alt="d" src="https://github.com/user-attachments/assets/fa601ea8-4751-461c-b137-5318ef7b8ead" />

**업로드 성공 시 결과**
<img width="1170" height="372" alt="연동성공" src="https://github.com/user-attachments/assets/6ffa1d1d-a0a8-43fa-bed7-7ef8716672e2" />

실제로 연동이 된 모습을 확인할 수 있다.

<img width="1354" height="488" alt="연동성공1" src="https://github.com/user-attachments/assets/84e4cb77-7f02-4c77-8255-e4082c16e050" />










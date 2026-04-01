# 개요
docker 컨테이너용 Nexus 설치

## 1. 설치
### 볼륨 생성
$ docker volume create nexus-data

### 컨테이너 생성
$ docker run -d --name nexus -p 8081:8081 -v nexus-data:/nexus-data sonatype/nexus3

### 생성 확인
$ docker ps -a | grep -i nexus

### nexus admin 비밀번호 확인 및 실행
(1) 비밀번호 확인
```
$ docker exec -it nexus /bin/sh
# cat /nexus-data/admin.password
02894ef2-e3f6-48c0-933f-49ec67beb117 (초기비밀번호)
```
(2) 브라우저에서 아래 url 입력
   http://localhost:8081 
   
(3) 접속 후 admin / (초기비밀번호) 입력 후 별도의 비밀번호로 변경

(4) 익명 접근은 불가하도록 설정

## 2. 환경 설정
### Blob Stores 설정
```
    Type: File
    Name: 스토리지명
    Path: 스토리지명에 따라 자동설정 됨
```
### Repositories
```
    - Select Recipe: docker (hosted)
    - Name: docker-hosted
	- HTTP: 5000
	- Check out "Allow anonymous docker pull ( Docker Bearer Token Realm required )"
	- Storage > Blob sotre: (1)에서 생성한 Blob 스토리지명 선택
```
### Security > Realms
```
    - docker login 명령어 실행하여 401 Unauthorized 발생시
	  Docker Bearer Token Realm을 Available에서 Active로 선택 후 저장
```
### 도커 이미지 업로드
```
$ docker tag Chanyong-Park/webflux:0.0.2 localhost:5000/chanyong/webflux:0.0.2
$ docker login http://localhost:5000
Username: 접속계정
Password: 접속비밀번호
Login Succeeded

$ docker push localhost:5000/chanyong/webflux:0.0.2
```

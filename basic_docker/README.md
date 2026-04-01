# 개요
스프링 부트 기반의 프로젝트를 docker 또는 docker-compose를 사용하여 컨테이너를 생성하고 관리하는 명령어

## 프로젝트
### 빌드
$ ./gradlew clean build

### 실행
$ ./gradlew bootRun

## docker 공통
### openjdk 17 컨테이너 이미지 다운로드
$ docker pull openjdk:17-jdk-slim

### 도커 컨테이너용 이미지 생성
$ docker build -t Chanyong-Park/webflux:0.0.2 -f Dockerfile .       ([Dockerfile 참조](https://github.com/Chanyong-Park/basic_docker/blob/main/Dockerfile))

### 도커 이미지 삭제
$ docker rmi Chanyong-Park/webflux:0.0.2

## docker를 이용한 컨테이너 생성
### 방법1
$ docker run -d -e WHICH_PROFILE=develop -p 8080:8081 --name webflux Chanyong-Park/webflux:0.0.2

### 방법2
$ docker create -e WHICH_PROFILE=develop -p 8080:8081 --name webflux Chanyong-Park/webflux:0.0.2

$ docker start webflux

### 중지
$ docker stop [컨테이너이름 또는 ID]

$ docker stop webflux


### 삭제
$ docker rm [컨테이너이름 또는 ID]

$ docker rm webflux

$ docker rm -f webflux #강제 중지 후 삭제


## docker-compose를 이용한 컨테이너 생성 ([docker-compose.yml 참조](https://github.com/Chanyong-Park/basic_docker/blob/main/docker-compose.yml))

### 도커 컴포즈 사용하여 컨테이너 생성
$ docker-compose -f docker-compose.yml up -d

### 도커 컴포즈 프로세스 확인
$ docker-compose ps

### 도커 컴포즈 사용하여 컨테이너 삭제
$ docker-compose -f docker-compose.yml down

$ docker compose -p webfluxByDockerCompose down

## 컨테이너 관리
### 로그보기
$ docker logs webfluxByDockerCompose

### 컨테이너 세부정보
$ docker inspect webfluxByDockerCompose

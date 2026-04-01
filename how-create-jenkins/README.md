# 개요
Jenkins 도커 컨테이너 설치

## 컨테이너 생성
### Jenkins 이미지 다운로드
```
$ docker pull jenkins/jenkins:lts-jdk17
```

### Jenkins 컨테이너 생성
```
$ docker run -d \
  --name jenkins \
  -p 8082:8080 \
  -p 50000:50000 \
  -v /var/jenkins_home:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -e TZ=Asia/Seoul \
  -u root \
  --restart=unless-stopped \
  jenkins/jenkins:lts-jdk17
```

### 컨테이너 내부에 docker 설치
```
$ docker exec -it jenkins /bin/bash
# apt-get update && apt-get install -y docker.io
# echo "172.17.0.2 nexus" | tee -a /etc/hosts
```

### 초기 비밀번호 확인
```
$ docker exec -it jenkins cat /var/jenkins_home/secrets/initialAdminPassword
d3da98c7f8ee4030a3fa82886a21ce87    #설치에 따라 달라질 수 있음
```

## Jenkins
### 환경설정 step by step
- Unlock Jenkins: http://localhost:8082 접속 후 초기 비밀번호 입력
- 기본 플러그인 설치
- 관리자 계정 생성
- 접속 URL 설정
- 추가 플러그인 설치
  pipeline, docker

```
$ docker login localhost:5000 # 컨테이너 안에서도 localhost로 해야 함
Username: 접속계정
Password: 접속비밀번호
Login Succeeded
```

### Jenkins item 생성
- pipeline 선택

``` script
pipeline {
    agent any

    stages {       
        stage('Checkout') {
            steps {
                git branch: 'master', 
                    url: 'https://github.com/codecentric/springboot-sample-app.git'
            }
        }        
        
        stage('Build') {
            steps {
                sh '''
                    #gradle wrapper                
                    chmod +x gradlew
                    ./gradlew clean build
                '''
            }
        }

        stage('upload Docker Image') {
            steps {
                sh '''
                    # 도커 로그인
                    echo "admin" | docker login localhost:5000 --username admin --password-stdin
                    # 이미지 생성

                    # 이미지 업로드
                '''
            }
        }
    }
}
```

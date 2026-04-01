# Java 17이 설치된 경량 Alpine 리눅스 이미지를 사용
FROM openjdk:17-jdk-slim

# 컨테이너 내 작업 디렉토리 지정
WORKDIR /app

# 빌드된 JAR 파일을 컨테이너로 복사
COPY build/libs/webflux-0.0.2-SNAPSHOT.jar app.jar

# 환경설정
ENV WHICH_PROFILE=local

# 컨테이너 노출 포트
EXPOSE 8080

# 컨테이너가 시작될 때 JAR 파일을 실행
ENTRYPOINT ["java", "-Dspring.profiles.active=${WHICH_PROFILE}", "-jar", "app.jar"]

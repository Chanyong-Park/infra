# 생성 및 실행 방법
## docker compose 실행 및 background로 container 생성
```
# docker-compose up -d
```

## redis 로그 확인
```
# docker-compose logs -f redis
```

## redis cli 접속
```
# docker exec -it redis_server redis-cli -a your_password
```

## redis insight / commander 브라우저 접속
```
http://localhost:8001 # Redis Insight
http://localhost:8081 # Redis Commander
```

## 중지
```
# docker-compose down
```

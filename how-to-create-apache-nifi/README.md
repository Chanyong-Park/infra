# how-to-create-apache-nifi

## git clone
```
git clone https://github.com/Chanyong-Park/how-to-create-apache-nifi.git
```

## image build
```
# docker build --progress=plain -t nifi-custom:v1.x.x . --no-cache
# docker tag nifi-custom:1.x.x nifi-custom:latest
```

## create tar file for copying to other system
```
# docker save -o nifi-custom.tar nifi-custom:latest      # 다른 서버로 복사하기 위해 tar파일 생성
# docker load -i nifi-custom.tar
# docker images
```

## create container
```
# docker compose up -d
```

## verify running container
```
# docker ps
CONTAINER ID   IMAGE                COMMAND                 CREATED         STATUS         PORTS                                         NAMES
f4798b94bac5   nifi-custom:latest   "../scripts/start.sh"   5 seconds ago   Up 4 seconds   0.0.0.0:8443->8443/tcp, [::]:8443->8443/tcp   nifi-server
```

## read logs
```
# docker logs -f nifi-server
```

## open in web browser
```
1. https://localhost:8443/nifi
2. login with id / password. You can get them from docker-compose.yml
```
### stop container
```
# docker compose down
# docker ps -a
```

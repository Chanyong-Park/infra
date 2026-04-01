# how-to-install-google-chrome-on-rockylinux9

## 도커 컨테이너 생성
```
$ docker pull rockylinux:9
$ docker run -d --name rocky9 rockylinux:9
```

## google-chrome-stable 의존성 download
```
$ docker exec -it rocky9 /bin/bash # container 안으로 진입
$ mkdir chrome-deps
$ dnf install 'dnf-command(download)' 
# 또는
$ dnf install dnf-plugins-core

$ cat << EOF > /etc/yum.repos.d/google-chrome.repo
[google-chrome]
name=google-chrome
baseurl=http://dl.google.com/linux/chrome/rpm/stable/x86_64
enabled=1
gpgcheck=1
gpgkey=https://dl.google.com/linux/linux_signing_key.pub
EOF

$ dnf download --resolve --alldeps google-chrome-stable --destdir ./chrome-deps
```
## chromedriver 의존성 download
```
$ dnf download --resolve --alldeps chromedriver --destdir ./chrome-deps
```

## 패키지 로컬 설치
```
$ cd chrome-deps
$ dnf localinstall -y ./*.rpm --skip-broken
# 또는
$ rpm -Uvh *.rpm
```

## 설치 확인
```
$ google-chrome --version
$ google-chrome --headless --no-sandbox --disable-gpu --dump-dom https://news.naver.com
```

## 의존성 파일 복사
```
$ exit # container 밖으로 탈출
$ docker cp [컨테이너ID]:/chrome-deps ./
```

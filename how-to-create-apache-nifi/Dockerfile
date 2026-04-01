FROM apache/nifi:latest

USER root

ENV PIP_BREAK_SYSTEM_PACKAGES=1

# 1. 필수 패키지 설치
RUN apt-get update && apt-get install -y \
    python3-pip \
    wget \
    gnupg \
    unzip \
    curl \
    && rm -rf /var/lib/apt/lists/*

# 2. Google Chrome 정식 버전 설치
RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - \
    && echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list \
    && apt-get update && apt-get install -y google-chrome-stable

# 3. 설치된 Chrome 버전에 맞는 Chromedriver 수동 설치
# 설치된 크롬의 메이저 버전을 확인하여 그에 맞는 드라이버를 가져옵니다.
RUN CHROME_MAJOR_VERSION=$(google-chrome --version | cut -d ' ' -f 3 | cut -d '.' -f 1) && \
    DRIVER_URL=$(curl -s "https://googlechromelabs.github.io/chrome-for-testing/last-known-good-versions-with-downloads.json" | \
    python3 -c "import sys, json; data = json.load(sys.stdin); \
    print([d['url'] for v in data['channels'].values() for d in v['downloads'].get('chromedriver', []) if d['platform'] == 'linux64'][0])") && \
    wget -q -O /tmp/chromedriver.zip "$DRIVER_URL" && \
    unzip /tmp/chromedriver.zip -d /tmp/ && \
    # 압축 해제 시 폴더가 생성될 수 있으므로 내부 파일을 찾아 이동
    mv /tmp/chromedriver-linux64/chromedriver /usr/local/bin/chromedriver && \
    chmod +x /usr/local/bin/chromedriver && \
    rm /tmp/chromedriver.zip

# 4. 파이썬 필수 라이브러리 설치
RUN pip3 install --upgrade pip && \
    pip3 install --no-cache-dir requests beautifulsoup4 selenium

# 5. 권한 설정 및 사용자 복구
RUN mkdir -p /opt/nifi/nifi-current/my-scripts && chown nifi:nifi /opt/nifi/nifi-current/my-scripts
USER nifi

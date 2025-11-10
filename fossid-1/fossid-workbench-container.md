# FossID Workbench Container 배포

## Workbench Containerized 애플리케이션 배포 가이드

![](https://fossid.mobis.co.kr/help/en/images/security.svg)

### <mark style="background-color:$info;">\[목차]</mark> <a href="#table-of-contents" id="table-of-contents"></a>

* [소개](fossid-workbench-container.md#introduction)
* [필수 조건](broken-reference)
* [이미지 관리](broken-reference)
* [배포 옵션](broken-reference)
* [구성 관리](fossid-workbench-container.md#configuration-management)
* [문제 해결](broken-reference)
* [프로덕션 배포](broken-reference)
* [추가 자료](broken-reference)

***

### <mark style="background-color:$info;">\[소개]</mark> <a href="#introduction" id="introduction"></a>

이 가이드는 컨테이너 환경에서 FossID Workbench를 배포하는 방법에 대한 자세한 지침을 제공합니다. 애플리케이션은 컨테이너 이미지로 배포되며 요구 사항에 따라 Docker, Podman 또는 Docker Compose를 사용하여 배포할 수 있습니다. 이 가이드에서는 필수 구성 요소, 구성, 배포 옵션, 문제 해결 및 모범 사례를 다룹니다.

#### 응용 프로그램 개요 <a href="#application-overview" id="application-overview"></a>

* **등록 위치** :`quay.io/fossid`
* **기본 이미지** :`workbench`
* **노출된 포트** :`80`
* **필수 환경 변수** : multiple
* **보관 요구 사항** :
  * fossid.conf
  * backup
  * intake
  * logs
  * uploads
  * scan-path
* **네트워크 구성** : bridge

***

### <mark style="background-color:$info;">\[필수 조건]</mark> <a href="#prerequisites" id="prerequisites"></a>

_FossID Workbench를_ 컨테이너로 배포하려면 필수 소프트웨어 중 하나를 사용할 수 있습니다.&#x20;

소프트웨어가 올바르게 설치되고 최신 버전으로 업데이트되었는지 확인하세요.

#### 필수 소프트웨어 <a href="#required-software" id="required-software"></a>

Docker 설치 확인

```
docker --version
```

Podman을 사용하는 경우 설치를 확인하세요.

```
podman --version
```

Docker Compose를 사용하는 경우

```
docker-compose --version
```

#### 인증 <a href="#authentication" id="authentication"></a>

_Quay.io의 FossID 개인 저장소_ 에 연결하려면 배달 포털에 제공된 사용자 이름과 비밀번호를 사용하세요 .

```
docker login quay.io
```

***

### <mark style="background-color:$info;">\[구성 관리]</mark> <a href="#configuration-management" id="configuration-management"></a>

#### 환경 변수 <a href="#environment-variables" id="environment-variables"></a>

* 로컬 개발을 위한 `.env` 파일 사용
* 외부 비밀 관리 솔루션(예: HashiCorp Vault, AWS Secrets Manager) 사용을 고려하세요.

`.env` 파일 의 예 :

```
PROJECT_NAME=deployment

# FOSSID Release configuration
# RELEASE=25.1.0
RELEASE=FOSSID_VERSION

# Database server environment configuration
MYSQL_ROOT_PASSWORD=root123
MYSQL_USER=fossiduser
MYSQL_PASSWORD=fossidpassword
MYSQL_HOST=db
FOSSID_MYSQL_DATABASE=fossid

# Web Server Configuration
# change only if default HTTP is modified or HTTPS is configured
# HTTPS_PORT=
HTTP_PORT=8081

# FossID Shinobi service configuration
FOSSID_SHINOBI_JAVA_ARGS="-Xms8g -Xmx8g -XX:NewSize=5g -XX:MaxNewSize=5g -XX:SurvivorRatio=8"
FOSSID_SHINOBI_ARGS="-threads 8 -timeout 275 -socket 127.0.0.1:9900"

# FossID Workbench
# The initial password for the `fossid` user.
FOSSID_INITIAL_PASSWORD=fossidlogin
```

#### FossID Workbench 구성 <a href="#fossid-workbench-configuration" id="fossid-workbench-configuration"></a>

FossID _Workbench_ 구성은 `fossid.conf` 파일을 통해 수행됩니다. 이미지에는 예제 파일(`fossid.conf.dist`)이 포함되어 있으며, 디렉토리는 `/fossid/etc/` 입니다.&#x20;

다음 명령을 실행하여 압축을 풀 수 있습니다. \
: `docker cp <container id>:/fossid/etc/fossid.conf.dist /path/on/host/fossid.conf.dist`.

필요한 최소 구성은 다음과 같습니다.

```
[CLI]
cli_server_host = FOSSID_HOST
cli_token = FOSSID_TOKEN
cli_strip_tags = .html

[WebApp]
webapp_db_server = MYSQL_HOST
webapp_db_database = MYSQL_DATABASE
webapp_db_username = MYSQL_USER
webapp_db_password = FOSSID_MYSQL_PASSWORD
webapp_db_port = 3306
webapp_server_name = fossid-workbench
webapp_timezone = Europe/Berlin
webapp_intake_repository =/fossid/intake/
webapp_backups=/fossid/backup/

webapp_base_url=https://fossid-workbench/index.php
```

예를 들어, docker 또는 podman 명령에 다음 옵션을 추가하여 파일을 컨테이너에 마운트할 수 있습니다.

```
  --volume ./fossid.conf:/fossid/etc/fossid.conf:Z
```

또는 다음과 같이 `compose.yaml` 파일에 볼륨으로 추가합니다.

```
...
  fossid-workbench-service:
  ...
    volumes:
      - ./fossid.conf:/fossid/etc/fossid.conf:Z
...
```

#### 포트 매핑 <a href="#port-mapping" id="port-mapping"></a>

애플리케이션은 **80번** 포트에 노출되어 있으며 , 이는 모든 수신 HTTP 트래픽이 컨테이너로 전달됨을 의미합니다. **Docker Compose** 에서는 다음과 같이 구성됩니다.

```
ports:
  - "80:80"
```

#### 볼륨 관리 <a href="#volume-management" id="volume-management"></a>

다음 디렉토리는 컨테이너 외부에서 데이터를 지속시키기 위해 볼륨이나 바인드 마운트로 구성할 수 있습니다.

```
# Create volumes
docker volume create fossid-wb-backup
docker volume create fossid-wb-intake
docker volume create fossid-wb-logs
docker volume create fossid-wb-uploads

# Inspect volumes
docker volume inspect fossid-wb-backup
docker volume inspect fossid-wb-intake
docker volume inspect fossid-wb-logs
docker volume inspect fossid-wb-uploads
```

**FossID Workbench** 에서 **target path** 옵션을 사용하려면 바인드 마운트된 볼륨을 사용하는 것이 좋습니다. Docker 또는 Podman에서 `--volume` 플래그를 사용하거나 `--mount` 플래그를 사용할 수 있습니다.&#x20;

다음은 몇 가지 예입니다.

```
# Create directory on the host
mkdir /path/to/target-path-directory
```

다음 옵션 중 하나를 사용하세요.

```
--volume /path/to/target-path-directory:/target-path:rw,Z
```

또는

```
--mount type=bind,source=/path/to/target-path-directory,target=/target-path
```

***

### <mark style="background-color:$info;">\[이미지 관리]</mark> <a href="#image-management" id="image-management"></a>

#### 이미지 끌어오기 <a href="#pulling-the-image" id="pulling-the-image"></a>

FossID 개인 저장소 quay.io에서 이미지를 가져옵니다.

```
docker pull quay.io/fossid/workbench:latest
```

또는 특정 버전

```
docker pull quay.io/fossid/workbench:25.1.0
```

특정 플랫폼 사용(필요한 경우)

```
docker pull --platform linux/amd64 quay.io/fossid/workbench:latest
```

#### 이미지 검증 <a href="#image-verification" id="image-verification"></a>

```
# Verify image presence
docker images | grep workbench

# Inspect image details
docker inspect --format='{{.RepoDigests}}' quay.io/fossid/workbench:25.1.0
```

***

### <mark style="background-color:$info;">\[배포 옵션]</mark> <a href="#deployment-options" id="deployment-options"></a>

#### 기본 Docker 배포 <a href="#basic-docker-deployment" id="basic-docker-deployment"></a>

```
docker network create --driver=bridge wb-network

docker run -d \
  --name db \
  --platform linux/amd64 \
  --env-file .env \
  --health-cmd 'mysqladmin -u"$MYSQL_USER" -p"$MYSQL_PASSWORD" -h127.0.0.1 ping' \
  --health-interval 5s \
  --health-timeout 5s \
  --health-retries 60 \
  --network wb-network \
  -v ./db/db-persist:/var/lib/mysql \
  docker.io/library/mysql:8.0

docker run -d \
  --name fossid-workbench \
  --platform linux/amd64 \
  --memory-reservation 8g \
  --tty \
  --env-file .env \
  --cap-add=CAP_AUDIT_WRITE \
  --health-cmd 'curl --silent --fail localhost:80/health-check' \
  --health-interval 5s \
  --health-timeout 5s \
  --health-retries 60 \
  --network wb-network \
  -p 8080:80 \
  -v ./fossid.conf:/fossid/etc/fossid.conf:Z \
  -v ./backup:/fossid/backup:Z \
  -v ./intake:/fossid/intake:Z \
  -v ./logs:/fossid/logs:Z \
  -v ./uploads:/fossid/uploads:Z \
  -v ./scan-path:/fossid/scan-path:Z \
  --restart unless-stopped \
  quay.io/fossid/workbench:latest
```

#### Docker Compose 배포 <a href="#docker-compose-deployment" id="docker-compose-deployment"></a>

&#x20;`docker-compose.yml` 생성:

```
services:
  fossid-workbench:
    image: quay.io/fossid/workbench:${RELEASE}
    platform: linux/amd64
    mem_reservation: 8g
    container_name: fossid-workbench
    ports:
      - "8081:80"
    tty: true
    env_file:
      - .env
    depends_on:
      - fossid-mysql
    healthcheck:
      test: curl --silent --fail localhost:80/health-check
      interval: 5s
      timeout: 5s
      retries: 60
    volumes:
      - ./fossid.conf:/fossid/etc/fossid.conf:Z
      - ./backup:/fossid/backup:Z
      - ./intake:/fossid/intake:Z
      - ./logs:/fossid/logs:Z
      - ./uploads:/fossid/uploads:Z
      - ./scan-path:/fossid/scan-path:Z
    networks:
      - wb-network
    cap_add:
      - CAP_AUDIT_WRITE

  fossid-mysql:
    image: docker.io/library/mysql:8.0
    platform: linux/amd64
    container_name: db
    env_file:
      - .env
    healthcheck:
      test: mysqladmin -u"$MYSQL_USER" -p"$MYSQL_PASSWORD" -h127.0.0.1 ping
      interval: 5s
      timeout: 5s
      retries: 60
    networks:
      - wb-network
    volumes:
      - ./db/db-persist:/var/lib/mysql

networks:
  wb-network:
    driver: bridge
```

다음과 함께 배포:

```
docker-compose up -d
```

#### Podman Pod 배치 <a href="#podman-pod-deployment" id="podman-pod-deployment"></a>

```
podman create network --driver=bridge wb-network

podman pod create --name fossid-workbench-pod --publish 8080:80 --network wb-network

podman run -d \
  --name db \
  --platform linux/amd64 \
  --pod fossid-workbench-pod \
  --env-file .env \
  --health-cmd 'mysqladmin -u"$MYSQL_USER" -p"$MYSQL_PASSWORD" -h127.0.0.1 ping' \
  --health-interval 5s \
  --health-timeout 5s \
  --health-retries 60 \
  -v ./db/db-persist:/var/lib/mysql \
  docker.io/library/mysql:8.0

podman run -d \
  --name fossid-workbench \
  --platform linux/amd64 \
  --pod fossid-workbench-pod \
  --memory-reservation 8g \
  --tty \
  --env-file .env \
  --cap-add=CAP_AUDIT_WRITE \
  --health-cmd 'curl --silent --fail localhost:80/health-check' \
  --health-interval 5s \
  --health-timeout 5s \
  --health-retries 60 \
  -p 8080:80 \
  -v ./fossid.conf:/fossid/etc/fossid.conf:Z \
  -v ./backup:/fossid/backup:Z \
  -v ./intake:/fossid/intake:Z \
  -v ./logs:/fossid/logs:Z \
  -v ./uploads:/fossid/uploads:Z \
  -v ./scan-path:/fossid/scan-path:Z \
  --restart unless-stopped \
  quay.io/fossid/workbench:latest
```

***

### <mark style="background-color:$info;">\[문제 해결]</mark> <a href="#troubleshooting" id="troubleshooting"></a>

#### 일반적인 문제 <a href="#common-issues" id="common-issues"></a>

컨테이너가 시작되지 않습니다.

```
# Check container status
docker ps -a

# View logs
docker logs [CONTAINER_ID]

# Check resource usage
docker stats
```

네트워크 연결 문제:

```
# Test network connectivity
docker network inspect [NETWORK_NAME]

# Enter container for debugging
docker exec -it [CONTAINER_ID] /bin/sh
```

볼륨 마운팅 문제:

```
# Verify mount points
docker inspect -f '{{ .Mounts }}' [CONTAINER_ID]
```

#### 디버깅 명령 <a href="#debugging-commands" id="debugging-commands"></a>

```
# Container shell access
docker exec -it [CONTAINER_ID] /bin/sh

# Process list in container
docker top [CONTAINER_ID]

# Resource usage
docker stats [CONTAINER_ID]

# Network information
docker network inspect [NETWORK_NAME]
```

***

### <mark style="background-color:$info;">\[Production 배포]</mark> <a href="#production-deployment" id="production-deployment"></a>

#### 보안 모범 사례 <a href="#security-best-practices" id="security-best-practices"></a>

이미지 보안:

```
# Scan for vulnerabilities
docker scan workbench:latest

# Use specific tags instead of 'latest'
docker pull quay.io/fossid/workbench@sha256:[DIGEST]
```

#### 역방향 프록시 설정(Nginx) <a href="#reverse-proxy-setup-nginx" id="reverse-proxy-setup-nginx"></a>

_Nginx 구성 예:_

```
server {
    listen 80;
    client_max_body_size 50M;
    location / {
        proxy_pass http://[APP_NAME]:[PORT];
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forward-Proto $scheme;
        proxy_read_timeout 10m;
        proxy_buffering off;
        proxy_request_buffering off;
        proxy_redirect http:// $scheme://;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_http_version 1.1;
        proxy_set_header Connection "";
    }
}
```

***

### <mark style="background-color:$info;">\[추가 자료]</mark> <a href="#additional-resources" id="additional-resources"></a>

* **Docker 문서**: https://docs.docker.com
* **Podman 문서:** https://docs.podman.io/
* **Docker Compose 문서**: https://docs.docker.com/compose

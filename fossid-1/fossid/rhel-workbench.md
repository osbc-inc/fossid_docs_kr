---
hidden: true
---

# RHEL의 Workbench 서버

### \[안내] <a href="#assumptions" id="assumptions"></a>

* 이 설치 지침에 따라 FossID Workbench는 `/fossid` 경로에 설치됩니다.
* 대상 운영 체제는 웹 서버나 SQL 서버 없이 설치됩니다.
* 설치 지침을 수행하여 로그인한 사용자는 `sudo`를 실행할 수 있습니다.

### \[OS 요구사항]

* RedHat Enterprise Linux 8 및 9
* RedHat Enterprise Linux Server 8 및 9

### \[PHP 요구사항]

* PHP의 최소 요구 버전은 <mark style="color:red;">**8.2**</mark>입니다.
* 단, 모든 하위 시스템(PHP, DB 등)은 **유지보수 중인 가장 오래된 버전 이상**을 사용하는 것을 권장합니다.

### \[DATABASE 요구사항]

* Database 서버의 최소 요구 버전은 <mark style="color:red;">**MySQL Server 8.0**</mark> <mark style="color:red;"></mark><mark style="color:red;">또는</mark> <mark style="color:red;"></mark><mark style="color:red;">**MariaDB 10.6**</mark>입니다.
* 단, 모든 하위 시스템(PHP, DB 등)은 **유지보수 중인 가장 오래된 버전 이상**을 사용하는 것을 권장합니다.

***

### <mark style="background-color:$info;">\[시스템 전체 설정에 대한 전제 조건]</mark> <a href="#prerequisites-on-system-wide-settings" id="prerequisites-on-system-wide-settings"></a>

#### <mark style="background-color:yellow;">1. 방화벽 포트 열기</mark> <a href="#open-firewall-ports" id="open-firewall-ports"></a>

```
sudo firewall-cmd --add-service=http --zone=public --permanent
sudo firewall-cmd --add-service=https --zone=public --permanent
```



#### <mark style="background-color:yellow;">2. SELINUX</mark> <a href="#selinux" id="selinux"></a>

SELINUX를 비활성화한 후 시스템을 재부팅하세요.

```
sudo vi /etc/selinux/config
```

FossID에는 아직 SELinux 정책이 없기 때문에 비활성화 필요합니다.



#### <mark style="background-color:yellow;">3. en\_US.UTF-8 Locale</mark> <a href="#en_usutf-8-locale" id="en_usutf-8-locale"></a>

Workbench를 사용하려면 호스트 환경의 locale에서 "en\_US.utf8"을 사용할 수 있어야 합니다.

시스템에서 현재 사용 가능한 locale을 표시하려면:

```
locale -a | grep en_US
```

"en\_US.utf8"이 없으면 추가해야 합니다.



#### <mark style="background-color:yellow;">4.  FossID Workbench에 필요한 패키지</mark> <a href="#packages-required-by-the-fossid-workbench" id="packages-required-by-the-fossid-workbench"></a>

<mark style="background-color:yellow;">**\[저장소 추가]**</mark>

**※ RedHat의 Epel 저장소**

Redhat 8의 경우

```
sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm -y
```

Redhat 9의 경우

```
sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm -y
```

\
**※ Remi의 RPM 저장소**

Redhat 8의 경우

```
sudo yum install -y https://rpms.remirepo.net/enterprise/remi-release-8.rpm
```

Redhat 9의 경우

```
sudo yum install -y https://rpms.remirepo.net/enterprise/remi-release-9.rpm
```



<mark style="background-color:yellow;">**\[패키지 설치]**</mark>

원하는 PHP 버전을 설정합니다(최소 필요 버전은 8.2입니다).

```
sudo yum module reset php
sudo yum module enable php:remi-8.2 -y
```

**레드햇 8**

```
sudo yum install bash bzip2 coreutils curl findutils git glibc grep gzip java-11-openjdk-headless \
            lbzip2 libxslt p7zip p7zip-plugins perl \
            php-cli php-curl php-fpm php-json php-ldap php-mbstring php-mysqlnd php-process php-xml php-zip php-intl \
            sudo tar unzip vim wget xz zip -y
```

**레드햇 9**

```
sudo yum install bash bzip2 coreutils curl findutils git glibc grep gzip java-11-openjdk-headless \
            lbzip2 libxslt p7zip p7zip-plugins perl \
            php-cli php-curl php-fpm php-json php-ldap php-mbstring php-mysqlnd php-process php-xml php-zip php-intl \
            sudo tar unzip vim wget xz zip -y
```

**참고:** Red Hat에서는 `unrar` 패키지가 기본 저장소에 포함되어 있지 않습니다.\
다만, 회사에서 `unrar` 패키지의 라이선스를 보유하고 있을 수 있습니다.\
FossID Workbench에서 RAR 파일을 추출해야 하는 경우, 시스템 관리자에게 `unrar` 패키지 보유 여부를 확인하십시오.

***

### <mark style="background-color:$info;">\[액세스 제공물]</mark> <a href="#access-deliverables" id="access-deliverables"></a>

FossID deliverables에 대한 접근 정보는 **delivery mail**에 포함되어 있습니다.

&#x20;delivery portal에서 `fossid-release_regular.x86_64.rpm`를 다운로드하세요 .

#### <mark style="background-color:yellow;">FossID 제공물 설치</mark> <a href="#install-fossid-deliverable" id="install-fossid-deliverable"></a>

FossID 설치:

<pre><code><strong>sudo yum localinstall fossid-release_regular.x86_64-{VERSION}.rpm -y
</strong></code></pre>

***

### <mark style="background-color:$info;">\[데이터베이스 및 웹 서버 설치]</mark> <a href="#database-and-web-server-installation" id="database-and-web-server-installation"></a>

#### <mark style="background-color:yellow;">1. MySQL/MariaDB 설치</mark> <a href="#install-mysqlmariadb" id="install-mysqlmariadb"></a>

```
sudo yum install -y mariadb mariadb-server
```

지원되는 시스템에서 MariaDB 버전이 이전 버전이므로 최신 버전을 설치하는 것이 좋습니다.&#x20;

MySQL 8.0 이상 버전을 설치하려면, [https://dev.mysql.com/doc/refman/8.0/en/installing.html](https://dev.mysql.com/doc/refman/8.0/en/installing.html) 의 공식 가이드를 따르는 것이 좋습니다.&#x20;

MariaDB 10.6 이상 버전을 설치하려면, [https://mariadb.com/kb/en/yum/](https://mariadb.com/kb/en/yum/) 의 공식 가이드를 따르는 것이 좋습니다 .

참고: MySQL/MariaDB 구성 파일에서 문자 집합 및 정렬에 대한 다음 값을 명시적으로 설정하는 것이 좋습니다.

```
character-set-server     = utf8mb4
collation-server         = utf8mb4_general_ci
```

MySQL 복제의 경우, 매개변수  `default_collation_for_utf8mb4` 는  `utf8mb4_general_ci` 로 설정해야 합니다 .&#x20;

자세한 내용은 [https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar\_default\_collation\_for\_utf8mb4](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_default_collation_for_utf8mb4)를 참조하세요.\


#### <mark style="background-color:yellow;">(1) 서버 구성 업데이트</mark> <a href="#update-server-configuration" id="update-server-configuration"></a>

MySQL 서버 배포판의 해당 파일(예: `/etc/mysql/my.cnf`  또는`/etc/my.cnf`)에서 `max_allowed_packet` 에 대해  `64M` 이상의 값을 `[mysqld]` 태그 아래에 설정해야 합니다.

아래를 참조하세요.

```
[mysqld]
max_allowed_packet = 64M
```

이는 Linux 배포판과 MySQL 서버 배포판에 따라 다를 수 있습니다. 해당 Linux 및 MySQL 버전 배포판의 설명서를 참조하세요.



데이터베이스 서비스를 시작하고 활성화합니다.

```
sudo systemctl enable --now mysqld.service
```

또는

```
sudo systemctl enable --now mariadb.service
```



<mark style="color:$info;">(참고) MySQL 구성</mark>

* 데이터베이스 : `fossid_db`
* 사용자 : `fossiduser`,   비밀번호 : `123`
* `fossiduser`에게 `fossid_db` 대한 액세스를 제공
* 설치 이후, Workbench 접속 초기 사용자 \[ 아이디 : `fossid` , 비밀번호 : `fossidlogin`  ]

위 구성은 나중에 `fossid.conf` 파일의 `webapp_db_*` 에 추가해야 합니다. \
강력하고 고유한 비밀번호를 사용하세요.



<mark style="background-color:yellow;">**\[MySQL 인스턴스 설정]**</mark>

데이터베이스를 생성합니다:

```
sudo mysql -h localhost -e "CREATE DATABASE fossid_db CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;"
```

\
사용자를 생성합니다.

```
sudo mysql -h localhost -e "CREATE USER 'fossiduser'@'localhost' IDENTIFIED BY '123';"
sudo mysql -h localhost -e "GRANT ALL PRIVILEGES ON fossid_db.* TO 'fossiduser'@'localhost' WITH GRANT OPTION;"
```

\
사용하는 서버가 (MariaDB가 아닌) MySQL 서버인 경우 다음 추가 명령도 실행하세요.

```
sudo mysql -h localhost -e "ALTER USER 'fossiduser'@'localhost' identified by '123';"
```

\
일부 시스템에서는 MariaDB가 mysql-server 패키지와 함께 설치됩니다. MySQL 서버가 설치되어 있는지 확인하려면 다음을 실행하세요.

```
mysql --version  
```

<mark style="color:$info;">→ MySQL이 설치된 경우의 출력 예:</mark>

```
mysql  Ver 8.0.35 for Linux on x86_64
```

<mark style="color:$info;">→ MariaDB가 설치된 경우의 출력 예:</mark>

```
mysql  Ver 15.1 Distrib 10.6.15-MariaDB, for debian-linux-gnu (x86_64) using readline 5.2
```

\
새로 만든 데이터베이스로 FossID 데이터베이스 스키마를 가져옵니다.

```
sudo mysql -u fossiduser -p'123' fossid_db < /fossid/setup/database/dbclean.sql
```



<mark style="background-color:yellow;">**\[관리자 비밀번호 구성]**</mark>

Workbench FossID 계정 관리자 비밀번호를 설정합니다. \
(처음 로그인할 때 비밀번호는 argon2id와 md5 해시가 제거된 상태로 해시 됩니다.)

```
mysql -h localhost -u fossiduser -e "update users set password_md5=md5('fossidlogin');" fossid_db -p'123'
```



#### <mark style="background-color:yellow;">2. 웹 서버 설치</mark> <a href="#install-web-server" id="install-web-server"></a>

이 참조 설정에서는 NginX 웹 서버를 사용합니다. \
다른 웹 서버도 사용 가능하지만, FossID는 NginX를 사용하므로, 본 문서에서는 NginX 기준으로 가이드 및 설정을 제공합니다.

Nginx 설치:

```
sudo yum install nginx -y
```



<mark style="background-color:yellow;">**(1) NginX 구성**</mark>

샘플 `/fossid/setup/templatesnginx.conf.dist`을  `/etc/nginx/nginx.conf` 로 복사합니다 :

```
sudo cp /fossid/setup/templates/nginx.conf.dist /etc/nginx/nginx.conf
```

기본적으로 NginX는 PHP 요청을 php8.2 소켓으로 전달하도록 설정되어 있습니다. \
다른 버전의 PHP가 설치되어 있는 경우 소켓 경로를 변경해야 합니다.

\
설치된 php 버전을 확인하려면 다음을 실행하세요.

```
php --version
```

\
php 8.2가 아닌 경우, `/etc/nginx/nginx.conf`을 편집하여 다음 섹션을 찾으세요.

```
location = /index.php {
    # If any other php version than 8.2 is used, please update this path
    fastcgi_pass unix:/run/php/php8.2-fpm.sock;
    include fastcgi_params;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    fastcgi_param SCRIPT_NAME $fastcgi_script_name;
}
```

`fastcgi_pass unix:/run/php/php8.2-fpm.sock;`부분에서 php의 올바른 버전으로 변경하세요.

예를 들어, php 버전이 8.3라면 다음과 같이 변경이 필요합니다.

```
fastcgi_pass unix:/run/php/php8.3-fpm.sock;
```



<mark style="color:$info;">(선택 사항) HTTPs 활성화</mark>

HTTP를 활성화하는 방법에 대한 지침은 `nginx.conf` 템플릿 파일에서 확인하세요 .

```
# How to enable ssl:
#   1. Comment the line above
#   2. generate a ssl certificate
#   3. Uncomment the following 4 lines
#   4. Update the paths for your .crt and .key file below
#   5. Update the server_name to match your servers domain name below
# ssl_certificate /etc/ssl/certs/nginx-selfsigned.crt;
# ssl_certificate_key /etc/ssl/private/nginx-selfsigned.key;
# listen 443 default_server ssl;
# server_name fossid.yourdomain.com;

# it is also recommended to generate a custom dhparam.pem file by running the command
# openssl dhparam -out /etc/nginx/dhparam.pem 2048
# ssl_dhparam /etc/nginx/dhparam.pem;
```



<mark style="background-color:yellow;">**(2) PHP 구성**</mark>

`/run/php` 디렉토리가 없으면 생성합니다 .

```
sudo mkdir -p /run/php
```

시스템이 부팅 시 /run/php 디렉토리가 존재하는지 확인하려면, 다음 내용을 포함하는 `/usr/lib/tmpfiles.d/php.conf` 파일을 만드세요.

```
d /run/php 0755 root root -
```

Linux 배포판에 해당하는 `www.conf`  파일을 편집 하고, 다음 구성이 설정되어 있는지 확인하거나 샘플 파일(`/etc/php/X.Y/fpm/pool.d/www.conf`)을 Linux 배포판의 해당 위치로 복사합니다.\
(`www.conf`  파일 위치 : `/etc/php-fpm.d/www.conf` 또는 `/etc/php/X.Y/fpm/pool.d/www.conf`)&#x20;

```
user = www-data
group = www-data
listen = /run/php/php8.2-fpm.sock
listen.owner = www-data
listen.group = www-data
listen.mode = 0660
;listen.acl_users = apache,nginx   <-- make sure it's commented out.
```



PHP의 올바른 버전을 확인한 후, 해당 버전에 맞게 `listen = /run/php/php<버전>-fpm.sock` 항목을 수정합니다.

예를 들어, php 버전이 8.3라면 다음과 같은 줄을 작성해야 합니다.

```
listen = /run/php/php8.3-fpm.sock
```

phpX.Y-fpm 서비스가 실행 중이고 www-data 사용자가 접근할 수 있는지 확인하세요. 일부 시스템에서는 서비스 이름이 php-fpm과 다를 수 있습니다.

\
그룹 소유권을 변경한 `/var/lib/php`후 php-fpm을 다시 시작합니다.

```
sudo chgrp www-data -R /var/lib/php/
sudo systemctl restart php-fpm
```

\
php-fpm 서비스를 시작합니다:

```
sudo systemctl start php-fpm
sudo systemctl enable php-fpm
```

\
NginX 서비스를 다시 시작합니다.

```
sudo systemctl restart nginx
sudo systemctl enable nginx
```

\
php 서비스 폴더의 그룹 소유권을 변경합니다.

```
sudo chgrp www-data -R /var/lib/php
```

***

### <mark style="background-color:$info;">\[FossID 구성]</mark> <a href="#configure-fossid" id="configure-fossid"></a>

#### <mark style="background-color:yellow;">\[기본 fossid.conf 설정]</mark> <a href="#basic-fossidconf-settings" id="basic-fossidconf-settings"></a>

FossID 구성 파일은 `/fossid/etc/fossid.conf`에 있습니다.



**※ 스캔 서버 액세스 구성**

```
cli_server_host = YOUR_SERVER_HOST
cli_token = YOUR_TOKEN
```



**※ 데이터베이스 연결 구성**

```
; Database server host
webapp_db_server=localhost

; Database server port
webapp_db_port=3306

; Database name
webapp_db_database=fossid_db

; Database user
webapp_db_username=fossiduser

; Database user password
webapp_db_password=123
```



**※  Workbench URL 구성**

이 정보는 이메일에서 올바른 절대 URL을 생성하는 데 사용됩니다.\
(https://mycompany.com 부분은 실제 사용 중인 FossID 서버의 접속 URL로 바꾸어 입력합니다.)

```
webapp_base_url = https://mycompany.com/index.php
```

\
fossid.conf 파일을 저장합니다.

(참고: 구성 변경 사항은 즉시 적용되므로 재시작이 필요하지 않습니다.)

***

#### <mark style="background-color:yellow;">\[설치 완료]</mark> <a href="#finalize-installation" id="finalize-installation"></a>

데이터베이스가 성공적으로 생성되었는지 확인하고 추가 인덱스를 추가합니다.

```
cd /fossid/setup/database
php dbupdate.php /fossid/etc/fossid.conf
```

필요한 역할과 권한을 만듭니다.

```
php db_info_update.php /fossid/etc/fossid.conf
```

라이선스 데이터베이스를 생성합니다.

```
php licenseupdate.php /fossid/etc/fossid.conf
```

***

#### <mark style="background-color:yellow;">\[Workbench 접근 확인]</mark> <a href="#verify-workbench-access" id="verify-workbench-access"></a>

[http://localhost/](http://localhost/) 로 이동합니다

_관리자 비밀번호 구성_ 단계에서 생성한 사용자 이름 `fossid`과 비밀번호로 로그인합니다 .

(참고: FossID Workbench는 공식적으로 Chrome 브라우저에서 지원됩니다.)

***

#### <mark style="color:$info;background-color:$info;">\[참고 사항]</mark> <a href="#configure-git" id="configure-git"></a>

#### <mark style="color:$info;">※ Git 구성</mark> <a href="#configure-git" id="configure-git"></a>

FossID Workbench는 Git 저장소에서 프로젝트 소스 코드를 직접 가져올 수 있는 API를 제공합니다. Workbench는 SSH를 통해 연결되며,  `www-data` 사용자가 사용할 수 있는 키가 필요합니다.

`www-data` 사용자 의 홈 디렉토리 경로를 확인하세요 .

```
cat /etc/passwd |grep www-data|cut -d : -f 6
```

출력은 `/var/www`와 유사합니다 .

\
홈 디렉토리에 .ssh라는 이름의 폴더를 만듭니다. (이전 명령의 출력이 `/var/www` 와 같다고 가정)

```
sudo mkdir /var/www/.ssh
```

새로 만든 .ssh 폴더에 git 서버에서 신뢰하는 개인 키를 복사하세요.

\
git 저장소를 호스팅하는 서버를 알려진 호스트에 추가해야 합니다. 추가하려는 각 서버에 대해 다음 명령을 실행하세요.

```
ssh-keyscan server_address >> /var/www/.ssh/known_hosts
```

\
www-data 사용자를 .ssh 폴더와 그 내용의 소유자로 지정합니다.

```
chown -R www-data:www-data /var/www/.ssh
```

\
Git 저장소를 사용하여 새 스캔을 생성하는 API 호출 방법은 제품 설명서를 확인하세요. 설명서는 메뉴(문서)에서 확인할 수 있으며, 다음 URL에서 확인할 수 있습니다.

[http://localhost/help/ko/index.html](http://localhost/help/en/index.html)\


#### <mark style="color:$info;">※ 종속성 분석 구성</mark> <a href="#configure-dependency-analysis" id="configure-dependency-analysis"></a>

FossID Workbench 사용자 인터페이스에서 바로 패키지 종속성 및 라이선스 정보를 제공하는 데 사용할 수 있는 FossID-DA 또는 OSS 검토 툴킷이라는 두 가지 도구가 있습니다. 종속성 분석 기능을 사용하면 소프트웨어가 준수해야 하는 라이선스에 대한 더 나은 통찰력을 얻을 수 있습니다. FossID는 종속성 분석 API도 제공하므로 지속적 통합 파이프라인에 포함할 수 있습니다.

[자세한 빌드 및 설치 지침은 종속성 분석 설치를](https://www.auditoss.kr/help/en/installation/dependency-analysis.html) 참조하세요 .\


#### <mark style="color:$info;">※ 스캔 용량 구성 - 클라이언트 측</mark> <a href="#configuring-scan-capacity---client-side" id="configuring-scan-capacity---client-side"></a>

클라이언트 측에서는 스캔이 발행되는 방식을 구성하여 토큰별보다 더 세부적인 수준에서 스캔 용량을 분배할 수 있습니다.

\
다음 설정은 단일 Workbench 스캔이 시작할 수 있는 스캔 스레드 수를 제어할 수 있습니다.

```
webapp_max_threads=8
```

\
동시 스캔 수는 다음 설정으로 제어할 수 있습니다.

```
webapp_max_concurrent_scans=3
```

새로운 스캔을 시작하려고 할 때 최대 스캔 수가 이미 진행 중이면 해당 스캔은 대기열에 추가되고 현재 실행 중인 스캔 중 하나가 완료되면 자동으로 시작됩니다.

\
간헐적인 네트워크 지연으로 어려움을 겪고 있다면 일괄 검사를 실행하면 전반적인 사용자 경험이 향상될 수 있습니다. 사용자 인터페이스에서 더 나은 사용자 경험을 얻으려면 아래 설정에서 일괄 처리 크기를 줄이세요. 네트워크 지연을 보완하려면 일괄 처리 크기를 늘리세요.

```
webapp_max_files_per_thread=16
```



문제가 발생할 경우 [문제 해결 페이지](undefined.md)를 참조하세요 .

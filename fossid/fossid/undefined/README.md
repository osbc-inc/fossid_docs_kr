# 업데이트 가이드

## Workbench Server 업데이트 지침

![](https://fossid.osbc.co.kr/help/en/images/security.svg)

### 가정 <a href="#assumptions" id="assumptions"></a>

* 현재 사용 중인 Workbench 버전은 <mark style="color:red;">22.2.x 이상</mark>입니다.\
  <mark style="color:red;">→ 만약 23.1 버전보다 이전 버전을 사용 중이라면, 먼저 22.2.x로 업데이트한 후 이 지침을 따르세요.</mark>\
  (23.1 버전 미만인 경우, 아래의  <mark style="color:green;">22.2로 업데이트하기 위한 선택적 준비</mark>를 참고하세요.)
* 업데이트대상 버전은 <mark style="color:red;">2025.1.0</mark>입니다.
* 기존 Workbench는 `/fossid` 디렉토리에 설치되어 있습니다.
* 데이터베이스 테이블 `permissions`와 `role_permission`은 수정되지 않았습니다.





### 패키지 변경 <a href="#package-changes" id="package-changes"></a>

<mark style="color:red;">버전 23.3부터</mark> FossID Workbench 로그를 rotate할 수 있는 기능이 추가되었습니다.



### OS 요구 사항 <a href="#os-requirements" id="os-requirements"></a>

FossID Workbench는 다음에서 지원됩니다.

* Debian 11
* Debian 12
* Ubuntu 20.04
* Ubuntu 22.04
* Ubuntu 24.04
* RedHat Enterprise Linux 8 and 9
* RedHat Enterprise Linux Server 8 and 9



* 필수 'PHP' 최소 버전 <mark style="color:red;">: PHP 8.2 이상</mark>
* 필수 '데이터베이스' 서버 최소 버전 :&#x20;
  * <mark style="color:red;">MySQL Server 8.0 또는 MariaDB 10.6</mark>
  * 단, 모든 하위 시스템은 아직 유지 관리(보안 및 업데이트 지원)가 이루어지고 있는 버전 이상을 사용하는 것이 좋습니다.





### 22.2로 업데이트하기 위한 선택적 준비 (선택사항)  <a href="#optional-preparation-for-updating-to-222" id="optional-preparation-for-updating-to-222"></a>

<mark style="color:green;">23.1 미만 버전을 사용 중이라면 해당 부분을 참고하세요.</mark>\


* 23.1.x 버전보다 이전 버전에서 22.2 또는 최신 버전으로 Workbench를 업데이트할 경우,\
  특정 데이터베이스 변경 작업 때문에 데이터베이스가 큰 경우 업데이트에 매우 오랜 시간이 걸릴 수 있습니다.  &#x20;
* 업데이트가 진행되는 동안에는 웹 애플리케이션을 사용할 수 없습니다.
* 업데이트를 빠르게 진행하려면, 업데이트 전에 직접 데이터베이스에 접속하여 아래 SQL 문장을 실행할 수 있습니다.

```
ALTER TABLE "file_client_results" MODIFY "file_license" mediumtext NULL;
```





### PHP 전제 조건 <a href="#php-prerequisites" id="php-prerequisites"></a>

*   만약 <mark style="color:red;">FossID 서버가 다음 중 하나의 운영체제에서 실행 중이라면,</mark>\
    <mark style="color:red;">PHP 8.2 이상을 설치할 수 있도록, 먼저</mark> <mark style="color:red;"></mark><mark style="color:red;">**PHP repository를 추가**</mark>해야 합니다.

    * **Debian 11**
    * **Ubuntu 20.04**
    * **Ubuntu 22.04**
    * **RedHat 8**
    * **RHEL Server 8**





**Debian 11 -** PHP 8.2 저장소 추가 및 설정

```
sudo apt update
sudo apt install lsb-release apt-transport-https ca-certificates -y
sudo wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/php8.2.list
sudo apt update
sudo apt remove php7\*
sudo apt remove php8.0\*
```

**Ubuntu 20.04 또는 Ubuntu 22.04 -** PHP 8.2 저장소 추가 및 설정

```
sudo add-apt-repository ppa:ondrej/php -y
sudo apt update
sudo apt remove php7\*
sudo apt remove php8.0\*
```

&#x20;Debian과 Ubuntu 모두에서, 위의 **`apt remove` 명령은 시스템에 설치된 정확한 PHP 버전에 따라 수정**되어야 할 수 있습니다.

> **참고:** 일부 Debian 또는 Ubuntu 시스템에서는 패키지 관리 방식 때문에, 사용 중인 PHP 버전에 필요한 모든 PHP 패키지들이 자동으로 설치되지 않을 수 있습니다.\
> -> 이런  경우에는 [문제 해결 페이지를](https://fossid.osbc.co.kr/help/en/installation/webapp_troubleshooting.html) 확인하세요 .



**RedHat 계열 시스템 (RedHat 8, RedHat 9)**

**RedHat의 \[Epel] repository**

**RedHat 9 -** Epel 저장소 추가

```
sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm -y
```

**RedHat 9 -** Epel 저장소 추가

```
sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm -y
```



**Remis의 \[RPM] repository**

**RedHat 8 -**  Remi 저장소 추가

```
sudo yum install -y https://rpms.remirepo.net/enterprise/remi-release-8.rpm
```

**RedHat 9 -**  Remi 저장소 추가

```
sudo yum install -y https://rpms.remirepo.net/enterprise/remi-release-9.rpm
```



**패키지 설치 -** PHP8.2 모듈 활성화 및 패키지 설치

원하는 PHP 버전을 설정합니다(<mark style="color:red;">최소 요구 버전은 8.2</mark>입니다):

```
sudo yum module reset php
sudo yum module enable php:remi-8.2 -y
```

**RedHat 8 -**&#xD544;수 패키지 설치

```
sudo yum install php-cli php-fpm php-process mariadb-server mariadb \
php-mysqlnd php-zip php-xml php-curl php-ldap php-mbstring php-intl \
vim libxslt unzip java-11-openjdk-headless git p7zip p7zip-plugins perl -y

```

**RedHat 9 -** 필수 패키지 설치

```
sudo yum install php-cli php-fpm php-process mariadb-server mariadb \
php-mysqlnd php-zip php-xml php-curl php-ldap php-mbstring php-intl \
vim libxslt

```

### MySQL/MariaDB 업그레이드

* MySQL 또는 MariaDB의 다양한 배포판에서 이전 버전이 사용되므로,\
  <mark style="color:red;">**최소 MySQL 8.0 이상**</mark> <mark style="color:red;"></mark><mark style="color:red;">또는</mark> <mark style="color:red;"></mark><mark style="color:red;">**MariaDB 10.6 이상**</mark><mark style="color:red;">으로 업그레이드하는 것을 권장</mark>합니다.
* 공식 설치 가이드는 다음 링크에서 확인할 수 있습니다.
  * MySQL 8.0: [https://dev.mysql.com/doc/refman/8.0/en/installing.html](https://dev.mysql.com/doc/refman/8.0/en/installing.html)
  * MariaDB 10.6: [https://mariadb.com/kb/en/binary-packages/](https://mariadb.com/kb/en/binary-packages/)



* MySQL/MariaDB 설정 파일에 다음과 같은 값을 명시적으로 설정하는 것이 좋습니다:

```
character-set-server     = utf8mb4
collation-server         = utf8mb4_general_ci
```



* MySQL Replication(복제)를 사용하는 경우에는 다음 매개변수도 설정해야 합니다:

```
default_collation_for_utf8mb4 = utf8mb4_general_ci
```

자세한 내용은 공식 문서를 참고하세요 : [https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar\_default\_collation\_for\_utf8mb4](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_default_collation_for_utf8mb4)



* MySQL 설정 파일 (`/etc/mysql/my.cnf` 또는 `/etc/my.cnf`)의 `[mysqld]` 섹션에 다음 항목을 추가하세요:

```
[mysqld]
max_allowed_packet = 64M
```

이 설정 파일의 위치는 리눅스 배포판과 MySQL 서버 버전에 따라 다를 수 있습니다. 각자의 문서를 참고하세요.



* MySQL을 다시 시작하세요:

```
sudo service mysql restart
```





### 액세스 제공물 <a href="#access-deliverables" id="access-deliverables"></a>

* FossID 전달물에 대한 접근 정보는 delivery mail에  포함됩니다.





### 패키지 설치 <a href="#install-the-package" id="install-the-package"></a>

**DEB 패키지 설치 (Ubuntu/Debian 계열**

```
sudo apt install ./fossid-release_regular_amd64-{VERSION}.deb -y
sudo systemctl restart php$(php -r 'echo PHP_MAJOR_VERSION.".".PHP_MINOR_VERSION;')-fpm.service
```

**RPM 패키지 설치 (RHEL/CentOS 계열)**

```
sudo yum localinstall ./fossid-release_regular.x86_64-{VERSION}.rpm -y
```







### fossid.conf 파일 수정 <a href="#update-fossidconf" id="update-fossidconf"></a>

* <mark style="color:red;">FossID Workbench 23.3 버전부터, Microsoft OAuth2 로그인을 사용했다면</mark> `fossid.conf` 파일에 아래와 같은 변경이 필요합니다:
  1. `webapp_microsoft_oauth2_login` → `webapp_oauth2_login` 으로 이름 변경
  2. `webapp_microsoft_oauth2_fallback_local_login` → `webapp_oauth2_fallback_local_login` 으로 이름 변경
  3. 다음 항목 추가 : `webapp_oauth2_provider = "Microsoft"`



* <mark style="color:red;">FossID Workbench 21.2 버전부터는 기존 fossid.conf 설정을 새로운 형식으로 마이그레이션할 필요가 없고, 권장되지도 않습니다.</mark>\
  그래도 마이그레이션을 원한다면 아래 절차를 따르세요&#x20;
  1. 기존 설정 백업 :  `cp /fossid/etc/fossid.conf /fossid/etc/fossid.conf_bak`
  2. 기본 템플릿으로 덮어쓰기: `mv /fossid/etc/fossid.conf.dist /fossid/etc/fossid.conf`
  3. 새 `fossid.conf` 파일을 열어 백업본과 비교하여 기존 설정을 반영합니다.







### **Workbench URL** 설정

* 이 설정은 이메일에 사용되는 절대 URL 생성을 위한 것입니다.

```
webapp_base_url = https://mycompany.com/index.php
```

fossid.conf 파일을 저장합니다.

(참고: 설정 파일 변경 사항은 저장 즉시 적용되며, 서비스 재시작은 필요하지 않습니다.)





### FossID 설치 후 작업 안내 (Post Install Actions)

### 데이터베이스 업데이트 <a href="#update-database" id="update-database"></a>

* <mark style="color:red;">**22.2 버전부터**</mark> <mark style="color:red;"></mark><mark style="color:red;">중복 데이터를 방지하기 위해 데이터베이스에</mark> <mark style="color:red;"></mark><mark style="color:red;">**UNIQUE 제약조건**</mark><mark style="color:red;">이 추가</mark>되었습니다.
* 기존 중복 항목을 식별하고 수정하려면 다음 명령을 /fossid/webapp 디렉토리에서 실행해야하고,\
  데이터 무결성 검사 명령어를 실행합니다.

```
cd /fossid/webapp
sudo -u www-data php bin/console.php enforce_data_integrity 
```



* **중복된 데이터**(라이선스, 컴포넌트 등)를 확인하고 정리할 수 있도록 **대화형 CLI**가 안내합니다.
* 예: 중복된 프로젝트/스캔/사용자 등은 이름에 접미사를 추가하여 구분하며, 식별 정보는 손실 없이 하나의 항목에 통합됩니다.
* 검사되는 테이블 목록 :&#x20;

```
The system checks if there is a duplicates in DataBase tables: 
'licenses'; 
'projects'; 
'components'; 
'scans'; 
'users'; 

```

-> 중복이 없을 경우 “UNIQUE CONSTRAINT를 추가하시겠습니까?”라는 메시지가 나오며, 기본값은 `yes`입니다. 이미 존재하는 경우는 무시해도 됩니다.

```
----------------------- 
The system didn't find duplicates. Do you want to add UNIQUE CONSTRAINTS to the tables? (defaults to yes)
  [0] yes
  [1] no
  > 0
```



* 가장 좋은 시나리오는 중복 항목이 발견되지 않고 고유 제약 조건을 직접 추가할 수 있다는 것입니다. \
  중복 항목이 발견되면 다음 메시지가 표시됩니다.

```
"The system found duplicates in the next tables:"
'projects'
'licenses'
----------------------- 
Do you want to clean duplicates? (defaults to yes)
  [0] yes
  [1] no
 > 0
```

* 다음 표와 열은 데이터의 고유성을 확인하는 데 사용됩니다.

```
  Table       unique by    Column(s)
 'projects'                'project_code' 
 'licenses'                'identifier' 
 'components'              'name' and  'version' 
 'scans'                   'code', 
 'users'                   'username'  
```

* 발견된 중복을 수정하려면 대화형 CLI에 표시되는 지침을 따르세요. \
  마지막 단계는 해당 테이블에 고유 제약 조건을 추가하는 것입니다. \
  고유 키가 이미 존재하는 경우 오류 메시지가 표시됩니다.

```
The system didn't find duplicates. Do you want to add UNIQUE CONSTRAINTS to the tables? (defaults to yes)
  [0] yes
  [1] no
 > 0
The system tries to add UNIQUE CONSTRAINT 'uc_identifier' to table: 'licenses' on column: `identifier`.
SQLSTATE[42000]: Syntax error or access violation: 1061 Duplicate key name 'uc_identifier'
```

고유 키가 이미 있으므로 다른 조치를 취할 필요가 없습니다.





* <mark style="color:red;">Release 20.1 버전 이전</mark>에 만들어진 컴포넌트 식별을 마이그레이션 하려면 :&#x20;
  * <mark style="color:red;">**20.1 이전 버전**</mark>에서는 파일당 하나의 컴포넌트만 식별 가능했으므로, 이전 데이터를 `identification_component` 테이블로 이전합니다.
  * <mark style="color:red;">**24.3 버전**</mark>에서는 기존의 `identifications.component_id` 컬럼이 제거됩니다.

```
sudo -u www-data php bin/console.php migrate_legacy_identifications --auto_migrate
```





* <mark style="color:red;">23.1.x 이전 버전에서 업그레이드 시 한 번 실행 필요 :</mark>&#x20;
  * `file_client_results` 테이블을 스캔별로 분리합니다.
  * 이렇게 하면 스캔 삭제 시 해당 테이블(`file_client_results_SCAN_ID`)을 바로 삭제하여 디스크 공간을 회수할 수 있습니다.
  * 새로 생성된 테이블이 있는지 꼭 확인하세요.
  * 기존 테이블은 `old_file_client_results`로 이름이 변경되어 남아 있습니다. 확인 후 수동으로 삭제 가능합니다.

중복 정리 및 `split_file_client_results` 실행 후 업그레이드 과정을 계속 진행하세요.

```
sudo -u www-data php bin/console.php split_file_client_results
```



* 데이터베이스 설정 확인 및 수동 수정

1. setup 폴더로 이동:

```
cd /fossid/setup/database
```

2. 모든 테이블의 `row_format`이 `DYNAMIC` 또는 `COMPRESSED`인지 확인하려면 다음 쿼리 실행:

```
SELECT table_name, row_format FROM information_schema.tables WHERE table_schema=DATABASE();
```

3. 필요하다면 아래 예시처럼 `ALTER TABLE` 문으로 수동 변경: \
   (아래 예제에서 DYNAMIC으로 설정되어 있습니다.)

```
ALTER TABLE [table_name] ROW_FORMAT=DYNAMIC;
```





* 인덱스 설정 및 데이터베이스 구조 확인

```
sudo -u www-data php dbupdate.php /fossid/etc/fossid.conf
```

* 권한과 역할 정보 확인 및 오래된 권한 제거

```
sudo -u www-data php db_info_update.php /fossid/etc/fossid.conf
```





* 라이선스 데이터베이스 업데이트

```
sudo -u www-data php licenseupdate.php --no-update --no-disable /fossid/etc/fossid.conf
```

인수에 주의하세요. `--no-update`및 `--no-disable`는 선택 사항입니다.

`--no-update` : 기본 데이터베이스 내 라이선스 텍스트 수정 유지

`--no-disable` : 수동 추가한 라이선스 또는 SPDX에서 deprecated 된 식별자 유지

`--no-fix-idents` : 나쁜 식별자 수정 방지 (권장하지 않음)

`--dry-run` : 변경 없이 실행 결과만 표시





* licenseupdate.php 매뉴얼 전체를 보려면 다음을 실행하세요.

```
php licenseupdate.php --help
```



* KB에서 제공하는 선택적 PURL 및 CPE 업데이트(Workbench 23.1부터)
  * 이미 데이터베이스에 존재하는 컴포넌트에 대해 KB에서 PURL과 CPE 정보를 채웁니다.
  * 로컬 컴포넌트 중 비어있는 CPE 필드도 함께 채워집니다.

<mark style="color:red;">Workbench 23.1에서 PURL 필드가 추가되었습니다</mark>. Workbench 데이터베이스에 이미 있는 구성 요소의 KB에서 이 필드를 채우려면 다음 명령을 실행하세요. 또한 로컬 구성 요소의 이 필드가 비어 있는 경우 이 명령은 KB에서 CPE 정보를 채웁니다.

```
cd /fossid/webapp
sudo -u www-data php bin/console.php add_purl_and_cpe
```





### Dependency 분석을 위한 OSS 리뷰 툴킷 업데이트 <a href="#update-oss-review-toolkit-for-dependency-analysis" id="update-oss-review-toolkit-for-dependency-analysis"></a>

* FossID Workbench 사용자 인터페이스 내에서 패키지 의존성 및 라이선스 정보를 제공하는 두 가지 도구가 있습니다: **FossID-DA** 와 **OSS Review Toolkit(ORT)**.
* 의존성 분석(Dependency Analysis) 기능을 이용하면, 소프트웨어가 준수해야 할 라이선스들을 더 잘 파악할 수 있습니다.&#x20;
* FossID는 의존성 분석 API도 제공하여, 이를 CI(지속적 통합) 파이프라인에 포함할 수 있습니다.



* FossID-DA 기본 도구 전환 (Workbench 24.3부터)
  * <mark style="color:red;">Workbench 24.3 버전부터 FossID-DA가 기본 의존성 분석 도구로 설정되었습니다.</mark>
  * FossID-DA에 관한 자세한 내용은 [여기의 관련 문서 섹션을 참조하세요.](https://fossid.osbc.co.kr/help/en/fda/FossID-DA-Getting-Started.html)



* OSS Review Toolkit (ORT) 업데이트 필요
  * 최근 ORT에 API 변경이 있었고, 현재 FossID Workbench 버전은 최신 ORT로 업데이트해야 정상 작동합니다.
  * 만약 이전 버전의 ORT를 설치할 때 배포 포털에서 설치 패키지를 사용했다면, 먼저 ORT를 제거하고 `/fossid/lib/ort` 폴더를 삭제해야 합니다. 그런 다음 배포 포털에서 새 ORT 버전(22.2)을 다운로드하여 설치하십시오.
  * ORT 제거 방법
    *   RHEL 계열&#x20;

        ```
        sudo yum remove fossid-ort
        sudo rm -rf /fossid/lib/ort
        ```
    *   Debian/Ubuntu 계열:

        ```
        sudo apt remove fossid-ort
        sudo rm -rf /fossid/lib/ort
        ```



소스 코드에서 ORT를 빌드한 경우 최신 변경 사항을 가져와서 다시 빌드하세요.



* 설치 관련 참고

자세한 내용은 "[Installation - Dependency Analysis](https://fossid.osbc.co.kr/help/en/installation/dependency-analysis.html)" 문서 섹션을 참고하세요.







### nginx 설정 변경 사항

* <mark style="color:red;">23.3.0 릴리즈에서 nginx 설정에 새로운 위치(location) 및 리라이트(rewrite)가 추가되었습니다.</mark>
* 현재 nginx 설정을 `/fossid/setup/templates/nginx.conf` 예제 파일과 비교하여 동기화하시기 바랍니다.

\
\




### PHP 업그레이드 시 필요한 변경 사항 <a href="#changes-required-when-upgrading-php" id="changes-required-when-upgrading-php"></a>

* PHP 버전을 업그레이드했다면 다음 사항을 꼭 확인하고 수정하세요.



1\) `/etc/nginx/nginx.conf` 파일 수정

아래 부분을 찾아서:

```
location = /index.php {
    # If any other php version than 8.2 is used, please update this path
    fastcgi_pass unix:/run/php/php8.2-fpm.sock;
    include fastcgi_params;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    fastcgi_param SCRIPT_NAME $fastcgi_script_name;
}
```

* `fastcgi_pass` 경로를 현재 사용 중인 PHP 버전에 맞게 수정합니다.
* 예를 들어 PHP 8.3을 쓴다면 다음과 같이 변경:

```
fastcgi_pass unix:/run/php/php8.3-fpm.sock;
```

* 변경 후 nginx를 재시작:

```
sudo service nginx restart
```



2\) `/etc/php-fpm.d/www.conf` 파일 수정

아래 설정들을 확인하고 수정하세요:

```
user = www-data
group = www-data
listen = /run/php/php8.2-fpm.sock
listen.owner = www-data
listen.group = www-data
listen.mode = 0660
;listen.acl_users = apache,nginx   <-- make sure it's commented out.
```

* listen 경로 역시 PHP 버전에 맞게 변경합니다. 예: PHP 8.3일 경우

```
listen = /run/php/php8.3-fpm.sock
```



3\) `/var/lib/php` 그룹 소유권 변경 및 php-fpm 재시작

```
sudo chgrp www-data /var/lib/php/*
sudo systemctl restart php-fpm
```

#### &#x20;<a href="#log-rotation" id="log-rotation"></a>



### 로그 회전(log rotation) 설정

* <mark style="color:red;">FossID Workbench 23.3 버전부터</mark> <mark style="color:red;"></mark><mark style="color:red;">`/fossid/logs`</mark> <mark style="color:red;"></mark><mark style="color:red;">폴더 내 로그 파일을 회전시키기 위한 설정 파일이 추가되었습니다.</mark>
* 설정 파일 위치는 `/etc/logrotate.d/fossid-workbench` 입니다.
* 필요에 따라 이 파일을 수정하여 로그 회전 정책을 조정할 수 있습니다.



### 오래된 crontab 관련 변경 사항 <a href="#remove-old-crontab-entries" id="remove-old-crontab-entries"></a>

* <mark style="color:red;">FossID 21.2 버전부터 백그라운드 작업(취약점 모니터링, 스캔 큐 관리 등)에 크론탭을 더 이상 사용하지 않습니다.</mark>
* 기존에 크론탭에 관련 작업이 등록되어 있었다면 다음 명령어로 편집해서 삭제하세요:

```
sudo crontab -e
```

* 삭제할 예시 항목:

```
* * * * *  /fossid/www/webapp/scans_queue_consumer.sh
0 1 * * *  /fossid/www/webapp/cpe_update_check.sh
0 0 * * *  /fossid/www/webapp/monitor.php --check v --n
```







### 선택사항: 화이트리스트(Whitelisting) 활성화 <a href="#optional-whitelisting" id="optional-whitelisting"></a>

* <mark style="color:red;">FossID 20.2 버전부터 기본적으로 화이트리스트 기능은 비활성화 상태입니다.</mark>
* 활성화하려면 `fossid.conf` 파일 내 `webapp_cli_command` 파라미터에 `--fields +mid` 옵션을 추가하세요. \
  \
  예:

```
webapp_cli_command=/fossid/bin/fossid-cli --config /fossid/etc/fossid.conf --fields +mid
```

주의: 화이트리스트 기능 활성화 이전에 스캔했던 파일은 재스캔이 필요합니다. 그래야 해당 파일에서 화이트리스트 규칙을 추가할 수 있습니다.

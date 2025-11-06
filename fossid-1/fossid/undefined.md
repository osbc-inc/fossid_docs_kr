---
hidden: true
---

# 문제 해결

### \[ 문제 해결 ] <a href="#troubleshooting" id="troubleshooting"></a>

#### ※ PHP - Workbench가 제대로 표시되지 않습니다. <a href="#php---workbench-not-showing-properly" id="php---workbench-not-showing-properly"></a>

고객 사이트에서 PHP가 제대로 설치되지 않은 사례가 보고되었습니다.\
Workbench에 예상대로 표시되지 않는 경우, 다음 항목이 올바르게 설치되어 있는지 확인하세요.

\
php가 실행 중인지 확인합니다.

```
ps -e |grep php
```

\
php가 실행 중인 경우, php.ini 파일이 로드되었는지 확인합니다:

```
php --ini
```

.ini 파일이 로드되어 있고 해당 파일에 콘텐츠가 포함되어 있는지 확인하세요.

\
문제가 해결되었다면 `php -m`을 실행하여 아래 모듈들이 설치되었는지 확인하세요:

* curl
* intl
* json
* ldap
* mbstring
* mysqli
* mysqlnd
* pdo\_mysql
* posix
* xml
* zip

\
다음 오류는 `php-intl`패키지가 설치되지 않았기 때문에 발생합니다.

```
Class "Locale" not found in file: /home/fossid/webapp/vendor/avadim/fast-excel-writer/src/FastExcelWriter/Excel.php, at line 295
```

\
Debian 및/또는 Ubuntu 시스템에서 PHP 업그레이드 시 구성이 손상될 수 있습니다. 업그레이드 과정에서 여러 버전의 PHP가 설치될 수 있습니다.&#x20;

**실행 중인 버전과 설치된 버전에 주의하고, 실행 중이거나 구성되지 않은 버전은 제거하십시오.**&#x20;

\
예를 들어 다음과 같은 시나리오가 있습니다.

* 시스템에는 php-8.2가 구성되어 있고 fossid-workbench-23.3.x가 있습니다.
* fossid-workbench-24.1.x로 업그레이드하면 php-8.3도 업데이트되지만, 이 버전의 php는 구성되어 있지 않습니다.
*   Debian/Ubuntu 시스템의 종속성 처리 방식으로 인해 일부 PHP 모듈이 누락될 수 있습니다. \
    \
    예를 들어 php-8.3이 설치되고, fossid-workbench-24.1에는 php-module-v1이 필요하며, php-8.2가 아닌 php-8.3용으로 설치됩니다. 제안된 해결책은 php-8.2 또는 php-8.3 중 하나의 PHP 버전을 제거하는 것입니다.\


    ```
    sudo apt remove php8.2\*
    ```

***

#### ※ MySQL 8은 `mysql_native_passwords` 사용에 대해 불만을 제기합니다. <a href="#mysql-8-complains-about-using-mysql_native_passwords" id="mysql-8-complains-about-using-mysql_native_passwords"></a>

일부 경우에는 이전 버전에서 mySQL 8로 업그레이드하면 로그에 다음과 같은 메시지가 표시됩니다:

```
Plugin mysql_native_password reported: ''mysql_native_password' is deprecated and will be removed in a future release. Please use caching_sha2_password instead'
```

\
MySQL 콘솔에서 `mysql_native_password` 플러그인을 사용하는 사용자를 확인하세요.

```
mysql> select User, plugin from mysql.user;
+------------------+-----------------------+
| User             | plugin                |
+------------------+-----------------------+
| fossid           | mysql_native_password |
| root             | mysql_native_password |
| mysql.infoschema | mysql_native_password |
| mysql.session    | mysql_native_password |
| mysql.sys        | mysql_native_password |
| root             | mysql_native_password |
+------------------+-----------------------+
6 rows in set (0.01 sec)
```

\
이 메시지를 제거하려면 사용자는 새로운 `caching_sha2_password`플러그인을 사용해야 합니다.

```
mysql> ALTER USER 'fossid'@'%' IDENTIFIED WITH caching_sha2_password BY 'MY_NEW_STRONG_PASSWORD';
```

***

#### ※ Timeout 관련 문제 <a href="#timeout-related-issues" id="timeout-related-issues"></a>

Nginx, PHP-FPM 및 PHP 설정에서 시간 초과 매개변수 값을 낮게 설정하면 다양한 문제가 발생할 수 있으며, 특히 장시간 실행되는 프로세스나 대용량 파일 업로드를 처리할 때 문제가 발생할 수 있습니다.&#x20;

이러한 경우 다음 값을 조정하는 것이 좋습니다.

**Nginx 구성( `nginx.conf`):**

```
send_timeout 1200;
fastcgi_read_timeout 1200;
fastcgi_send_timeout 1200;
fastcgi_connect_timeout 1200;
```

\
**PHP-FPM 구성( `www.conf`):**

```
request_terminate_timeout = 1200
```

\
**PHP 구성( `php.ini`):**

```
max_execution_time = 1200
```

\
그런 다음 이러한 변경 사항을 적용한 후, **서비스를 다시 시작하거나 다시 로드**합니다.

```
sudo systemctl restart php-fpm
sudo systemctl restart nginx
```

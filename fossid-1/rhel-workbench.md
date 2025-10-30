# RHEL의 Workbench 서버

### Assumptions <a href="#assumptions" id="assumptions"></a>

* Following these installation instructions, the FossID Workbench will be installed in `/fossid`.
* The target operating system is installed without Web server nor SQL server.
* The user logged in performing the installation instructions is allowed to run `sudo`.
* This example is applicable to:
  * RedHat Enterprise Linux 8 and 9
  * RedHat Enterprise Linux Server 8 and 9

The minimum PHP required version is 8.2.

The minimum required version of the database server is MySQL Server 8.0 or MariaDB 10.6.

However, we recommend that all sub-systems are at least in their oldest still maintained version.

### Prerequisites on system wide settings <a href="#prerequisites-on-system-wide-settings" id="prerequisites-on-system-wide-settings"></a>

#### Open firewall ports <a href="#open-firewall-ports" id="open-firewall-ports"></a>

```
sudo firewall-cmd --add-service=http --zone=public --permanent
sudo firewall-cmd --add-service=https --zone=public --permanent
```

#### SELinux <a href="#selinux" id="selinux"></a>

Please disable SELINUX and then reboot the system:

```
sudo vi /etc/selinux/config
```

This is needed as FossID do not yet have a SELinux policy in place.

#### en\_US.UTF-8 Locale <a href="#en_usutf-8-locale" id="en_usutf-8-locale"></a>

The Workbench requires “en\_US.utf8” to be available in the host environment’s locale.

To display current available locales on your system:

```
locale -a
```

If the “en\_US.utf8” is not present, it needs to be added.

#### Packages required by the FossID Workbench <a href="#packages-required-by-the-fossid-workbench" id="packages-required-by-the-fossid-workbench"></a>

**Add repositories**

**Epel repository on RedHat**

RedHat 8

```
sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm -y
```

RedHat 9

```
sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm -y
```

**Remi’s RPM Repository**

For Redhat 8

```
sudo yum install -y https://rpms.remirepo.net/enterprise/remi-release-8.rpm
```

For Redhat 9

```
sudo yum install -y https://rpms.remirepo.net/enterprise/remi-release-9.rpm
```

**Install packages**

Set the desired version of PHP (minimum required version is 8.2).

```
sudo yum module reset php
sudo yum module enable php:remi-8.2 -y
```

**RedHat 8**

```
sudo yum install bash bzip2 coreutils curl findutils git glibc grep gzip java-11-openjdk-headless \
            lbzip2 libxslt p7zip p7zip-plugins perl \
            php-cli php-curl php-fpm php-json php-ldap php-mbstring php-mysqlnd php-process php-xml php-zip php-intl \
            sudo tar unzip vim wget xz zip -y
```

**RedHat 9**

```
sudo yum install bash bzip2 coreutils curl findutils git glibc grep gzip java-11-openjdk-headless \
            lbzip2 libxslt p7zip p7zip-plugins perl \
            php-cli php-curl php-fpm php-json php-ldap php-mbstring php-mysqlnd php-process php-xml php-zip php-intl \
            sudo tar unzip vim wget xz zip -y
```

NOTE: In RedHat, unrar is not distributed in the standard repository. However, your company may have licensed the unrar package. If you need to extract rar files in FossID Workbench, ask your system administrator if the unrar package is available.

### Access Deliverables <a href="#access-deliverables" id="access-deliverables"></a>

Access information to the FossID deliverables is provided in the delivery mail.

Download `fossid-release_regular.x86_64.rpm` from the delivery portal.

#### Install FossID deliverable <a href="#install-fossid-deliverable" id="install-fossid-deliverable"></a>

Install FossID:

```
sudo yum localinstall fossid-release_regular.x86_64-{VERSION}.rpm -y
```

### Database and Web Server Installation <a href="#database-and-web-server-installation" id="database-and-web-server-installation"></a>

#### Install MySQL/MariaDB <a href="#install-mysqlmariadb" id="install-mysqlmariadb"></a>

```
sudo yum install -y mariadb mariadb-server
```

Due to the older version of MariaDB on the supported systems we recommend installing a newer version. For installing MySQL 8.0 or a newer version we recommend following the official guide at [https://dev.mysql.com/doc/refman/8.0/en/installing.html](https://dev.mysql.com/doc/refman/8.0/en/installing.html) For installing MariaDB 10.6 or a newer version we recommend following the official guide at [https://mariadb.com/kb/en/yum/](https://mariadb.com/kb/en/yum/).

NOTE: It is recommended to explicitly set these values for character set and collation in your MySQL/MariaDB config file:

```
character-set-server     = utf8mb4
collation-server         = utf8mb4_general_ci
```

For MySQL Replication the parameter `default_collation_for_utf8mb4` must be set to `utf8mb4_general_ci`. More details here: [https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar\_default\_collation\_for\_utf8mb4](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_default_collation_for_utf8mb4)

#### Update server configuration <a href="#update-server-configuration" id="update-server-configuration"></a>

A value of at least `64M` needs to be set for `max_allowed_packet` under `[mysqld]` tag in the corresponding file for your mysql server distribution (e.g. `/etc/mysql/my.cnf` or `/etc/my.cnf`). See below reference:

```
[mysqld]
max_allowed_packet = 64M
```

This may vary for each Linux distribution and mysql server distribution. Please review the documentation for your corresponding Linux and mysql version distribution.

Start and enable the database service:

```
sudo systemctl enable --now mysqld.service
```

or

```
sudo systemctl enable --now mariadb.service
```

**Configure MySQL**

In this example, we will:

* Create the database `fossid_db`
* Create user `fossiduser` with the password `123`
* Provide access to `fossid_db` for the `fossiduser`.
* Create Workbench user with user name `fossid` and password `fossidlogin`.

These credentials will later need to be added to the `webapp_db_*` configuration in the `fossid.conf` configuration file. Please use strong and unique passwords.

**Setup Mysql instance**

Create the database:

```
sudo mysql -h localhost -e "CREATE DATABASE fossid_db CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;"
```

Create the user:

```
sudo mysql -h localhost -e "CREATE USER 'fossiduser'@'localhost' IDENTIFIED BY '123';"
sudo mysql -h localhost -e "GRANT ALL PRIVILEGES ON fossid_db.* TO 'fossiduser'@'localhost' WITH GRANT OPTION;"
```

If the server you use is the MySQL server (not MariaDB), run this extra command as well:

```
sudo mysql -h localhost -e "ALTER USER 'fossiduser'@'localhost' identified by '123';"
```

Please note that on some systems, MariaDB is installed with the mysql-server package. To find out if MySQL server is installed, run:

```
mysql --version
```

Example output when MySQL is installed:

```
mysql  Ver 8.0.35 for Linux on x86_64
```

Example output when MariaDB is installed:

```
mysql  Ver 15.1 Distrib 10.6.15-MariaDB, for debian-linux-gnu (x86_64) using readline 5.2
```

Import FossID database schema to the newly created database:

```
sudo mysql -u fossiduser -p'123' fossid_db < /fossid/setup/database/dbclean.sql
```

**Configure Admin Password**

Set your Workbench FossID account admin password (at first login the password will be hashed with argon2id and md5 hash removed):

```
mysql -h localhost -u fossiduser -e "update users set password_md5=md5('fossidlogin');" fossid_db -p'123'
```

#### Install Web Server <a href="#install-web-server" id="install-web-server"></a>

In this reference set up, we will use the NginX webserver. You are free to use other webservers as well, though as FossID uses NginX, we can assist in configuration.

Install Nginx:

```
sudo yum install nginx -y
```

**Configure NginX**

Copy the sample `nginx.conf.dist` from `/fossid/setup/templates` to `/etc/nginx/`:

```
sudo cp /fossid/setup/templates/nginx.conf.dist /etc/nginx/nginx.conf
```

By default, NginX is configured to forward php requests to a php8.2 socket. If you have a different version of php installed, the path to the socket needs to be changed.

To find out, what version of php is installed, run:

```
php --version
```

If it is different than 8.2, edit the `/etc/nginx/nginx.conf` and look for the following section:

```
location = /index.php {
    # If any other php version than 8.2 is used, please update this path
    fastcgi_pass unix:/run/php/php8.2-fpm.sock;
    include fastcgi_params;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    fastcgi_param SCRIPT_NAME $fastcgi_script_name;
}
```

Change the `fastcgi_pass unix:/run/php/php8.2-fpm.sock;` to point to the right version of php. For example, if php version is 8.3, the line should look like this:

```
fastcgi_pass unix:/run/php/php8.3-fpm.sock;
```

**Enable HTTPs (optional)**

Find the instructions in the `nginx.conf` template file on how to enable HTTPs:

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

**Configure PHP**

Create `/run/php` directory if it does not exist:

```
sudo mkdir -p /run/php
```

To make sure the /run/php directory exists when system boots, create a file `/usr/lib/tmpfiles.d/php.conf` with the following contents:

```
d /run/php 0755 root root -
```

Edit the `www.conf` file corresponding to your Linux distribution (`/etc/php-fpm.d/www.conf` or `/etc/php/X.Y/fpm/pool.d/www.conf`) and make sure the following configuration is set, or copy the sample file from `/fossid/setup/templates/www.conf.dist` to the corresponding location of your Linux distribution:

```
user = www-data
group = www-data
listen = /run/php/php8.2-fpm.sock
listen.owner = www-data
listen.group = www-data
listen.mode = 0660
;listen.acl_users = apache,nginx   <-- make sure it's commented out.
```

Change the `listen = /run/php/php8.2-fpm.sock` to point to the right version of php. For example, if the php version is 8.2 the line should look like this:

```
listen = /run/php/php8.2-fpm.sock
```

Make sure that the phpX.Y-fpm service is running and accessible by www-data user. Please note that on some systems, the service name may be different (php-fpm).

Change the group ownership of `/var/lib/php` and then restart the php-fpm:

```
sudo chgrp www-data -R /var/lib/php/
sudo systemctl restart php-fpm
```

Start php-fpm service:

```
sudo systemctl start php-fpm
sudo systemctl enable php-fpm
```

Restart NginX service:

```
sudo systemctl restart nginx
sudo systemctl enable nginx
```

Change the group ownership of the php service folder:

```
sudo chgrp www-data -R /var/lib/php
```

### Configure FossID <a href="#configure-fossid" id="configure-fossid"></a>

#### Basic fossid.conf settings <a href="#basic-fossidconf-settings" id="basic-fossidconf-settings"></a>

The FossID configuration file is at `/fossid/etc/fossid.conf`.

**Configure Scan Server access**

```
cli_server_host = YOUR_SERVER_HOST
cli_token = YOUR_TOKEN
```

**Configure database connection**

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

**Configure Workbench URL**

This information is used to generate correct absolute URLs in emails:

```
webapp_base_url = https://mycompany.com/index.php
```

Save your fossid.conf file.

NOTE: The changes to configuration is immediate, no restart is required.

#### Finalize installation <a href="#finalize-installation" id="finalize-installation"></a>

Verify that the database was created successfully and add additional indexes:

```
cd /fossid/setup/database
php dbupdate.php /fossid/etc/fossid.conf
```

Create the required roles and permissions:

```
php db_info_update.php /fossid/etc/fossid.conf
```

Create the license database:

```
php licenseupdate.php /fossid/etc/fossid.conf
```

#### Verify Workbench Access <a href="#verify-workbench-access" id="verify-workbench-access"></a>

Browse to [http://localhost/](http://localhost/)

Login with user name `fossid` and the password that you created in the [Configure Admin Password](https://www.auditoss.kr/help/en/installation/webapp_rhel.html#configure-admin-password) step.

NOTE: FossID Workbench is officially supported on Chrome browser.

#### Configure Git <a href="#configure-git" id="configure-git"></a>

FossID Workbench provides the API allowing you to get a project source code directly from a git repository. The Workbench connects using SSH and it needs the keys to be available for the `www-data` user.

Check the path to the home directory for the `www-data` user:

```
cat /etc/passwd |grep www-data|cut -d : -f 6
```

The output will be similar to `/var/www`.

Create a folder named .ssh in the home directory (assuming the output of the previous command was `/var/www`):

```
sudo mkdir /var/www/.ssh
```

Copy the private key that is trusted by your git server in the newly created .ssh folder

The server hosting the git repository needs to be added to known hosts. For each server you want to add run this command:

```
ssh-keyscan server_address >> /var/www/.ssh/known_hosts
```

Make the www-data user the owner of the .ssh folder and its contents:

```
chown -R www-data:www-data /var/www/.ssh
```

Check the product documentation on how to make a API call to create a new scan using a git repository. The documentation is accessible from the menu (Docs) and available at this URL:

[http://localhost/help/en/index.html](http://localhost/help/en/index.html)

#### Configure Dependency Analysis <a href="#configure-dependency-analysis" id="configure-dependency-analysis"></a>

There are two tools: FossID-DA or OSS Review Toolkit that can be used to provide information on package dependencies and their license information right in the FossID Workbench user interface. Using the Dependency Analysis feature, you can get a better insight into the licenses your software needs to be compliant with. FossID also provides API for the Dependency Analysis so it can be included in your Continuous Integration pipeline.

See the [Dependency Analysis Installation](https://www.auditoss.kr/help/en/installation/dependency-analysis.html) for detailed build and installation instructions.

#### Configuring Scan Capacity - Client Side <a href="#configuring-scan-capacity---client-side" id="configuring-scan-capacity---client-side"></a>

The client side can configure how scans are issued, allowing scan capacity distribution on a on a more granular level than per token.

The following setting can control how many scanning threads a single workbench scan can initiate:

```
webapp_max_threads=8
```

The number of concurrent scans can be controlled with this setting:

```
webapp_max_concurrent_scans=3
```

If the max number of scans is already in progress when attempting to start a new scan that scan will be added to a queue and started automatically when one of the currently running scans has finished.

If you are suffering from intermittent network latencies, issuing batch scans may improve the overall experience. Decrease the batch size in the setting below, to get a better user experience in the user interface. Increase it to compensate for network latencies:

```
webapp_max_files_per_thread=16
```

If you are experiencing difficulties please see [troubleshooting page](troubleshooting.md).

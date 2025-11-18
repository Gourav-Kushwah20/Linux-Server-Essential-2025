
# PHP and MySQL Installation and Configuration

This guide covers the installation and configuration of PHP, MySQL, phpMyAdmin, WordPress, and Apache Virtual Hosts. Each step includes an explanation before executing the corresponding command.

## PHP Installation and Configuration

### Step 1: Install Required Repositories

Before installing PHP, we must enable the Extra Packages for Enterprise Linux (EPEL) repository and the Remi repository, which provides updated PHP versions.

Run the following command to install EPEL and YUM utilities:

```bash
yum install epel-release yum-utils mod_ssl
````

After installation, verify the available repositories by running:

```bash
yum repolist all
```

Visit the official Remi repository to get the appropriate repository package:
[Remi Repository](https://rpms.remirepo.net/wizard/)

Now, install the Remi repository for Enterprise Linux 9:

```bash
yum install http://rpms.remirepo.net/enterprise/remi-release-9.rpm
```

Verify that the repository has been added by listing all available repositories again:

```bash
yum repolist all
```

To check configuration files related to the `remi-release` package, run:

```bash
rpm -qc remi-release
```

To view all files installed by the `remi-release` package, use:

```bash
rpm -ql remi-release
```
If necessary,edit the repository configuration file manually:
```bash
vim /etc/yum.repos.d/rem1.repo
```
---
## Search for Available PHP Version
- Before installation PHP , check available PHP version in the repo.
```bash
yum search php
```
For more details information about the default PHP package ,run:
```bash
yum info php.x86_64
```
To check the details of PHP 8.1 and PHP 8.4
```bash
yum info php81.x86_64
yum info php84.x86_64
```
---
## Install PHP and Required Extensions

Once we have identified the PHP version to install, proceed with the installation of PHP and essential extensions. This command installs PHP and commonly used modules such as MySQL support, XML processing, and multibyte string handling.

```bash
yum install php php-common.x86_64 php-cli.x86_64 php-opcache.x86_64 php-gd.x86_64 php-curl php-mysqlnd.x86_64 php-xml.x86_64 php-mbstring.x86_64 php-pear php-mbstring php-pecl-http
````

### Verify PHP Installation

After installation, confirm that PHP has been successfully installed by checking the version:

```bash
php -v
```

### Step 5: Restart Apache

To apply PHP changes, restart the Apache web server:

```bash
systemctl restart httpd.service
```

### Step 6: Create a PHP Info Page

To verify that PHP is working correctly with Apache, create a `phpinfo.php` file inside the web root directory:

```bash
vim /var/www/html/phpinfo.php
```

Inside the file, add the following PHP script:

```php
<?php
phpinfo();
?>
```

Save the file and access it in your browser at:

    http://youserverIP/phpinfo.php

Example:- http://192.168.1.21/phpinfo.php

---

# MySQL Installation and Configuration

## Download and Install MySQL Repository

https://dev.mysql.com/downloads/

To install MySQL, first, download the official MySQL repository package:

```bash
wget https://dev.mysql.com/get/mysql80-community-release-el9-2.noarch.rpm
````

Now install the downloaded repository package:

```bash
yum install ./mysql80-community-release-el9-2.noarch.rpm
yum install https://dev.mysql.com/get/mysql84-community-release-el9-2.noarch.rpm
```

Verify that the MySQL repository has been added:

```bash
yum repolist all
```

## Enable and Disable MySQL Versions

### Enable MySQL 8.0 repository:

```bash
yum-config-manager --enable mysql80-community
```

### (Optional) Enable MySQL 5.7 repository if required:

```bash
yum-config-manager --enable mysql57.community
```

### If you mistakenly enabled MySQL 5.7, disable it:

```bash
yum-config-manager --disable mysql57.community
```
---

## Step 3: Search and Install MySQL

### Search for available MySQL packages:

```bash
yum search mysql
````

### Install MySQL server and development libraries:

```bash
yum install mysql-community-server mysql-community-devel
```

---

## Step 4: Start and Enable MySQL Service

### Start the MySQL service:

```bash
systemctl start mysqld.service
```

### Enable MySQL to start automatically on boot:

```bash
systemctl enable mysqld.service
```

### Check if MySQL is running:

```bash
systemctl status mysqld.service
```

---

## Step 5: Retrieve Temporary Root Password

After MySQL installation, a temporary root password is generated. Retrieve it using:

```bash
grep 'temporary password' /var/log/mysqld.log
```

---

## Step 6: Secure MySQL Installation

Run the MySQL secure installation script to set a new root password and remove insecure default settings:

```bash
mysql_secure_installation
```

**Example new password:**
```bash
P@ssword@123
```
skip > hint ENTER

- Now,Log into MySql using the new password
```bash
mysql -u root -p P@ssword@123
```
---
## Change Root Password(If Needed)
- If you want to manually update the MYSQL root password, log and run

```bash
ALTER USER root@localhost IDENTIFIED with mysql_native_password BY 'Armour@123';
SHOW DATABASES;
EXIT;
```
---
# phpMyAdmin Installation and Configuration

phpMyAdmin is a web-based tool used for managing MySQL databases. It provides an intuitive graphical interface for performing database operations without needing to use the command line.

---

## Download and Extract phpMyAdmin

First, navigate to the official phpMyAdmin website to get the latest version:

[phpMyAdmin](https://www.phpmyadmin.net)

Now, download the phpMyAdmin package:

```bash
wget https://files.phpmyadmin.net/phpMyAdmin/5.1.0/phpMyAdmin-5.1.0-all-languages.zip
````

Extract the downloaded package:

```bash
unzip phpMyAdmin-5.1.0-all-languages.zip
```

Move the extracted files to the Apache web directory:

```bash
mkdir /var/www/html/phpmyadmin
cp -vr phpMyAdmin-5.1.0-all-languages/* /var/www/html/phpmyadmin/
```

Navigate to the web root directory:

```bash
cd /var/www/html/
```
Change Ownership
```bash
chown -Rv apache:apache phpmyadmin
```
check ownership
```bash
ls -lh
```

---

## Step 2: Configure phpMyAdmin

Copy the sample configuration file to make it active:

```bash
cp /var/www/html/phpmyadmin/config.sample.inc.php /var/www/html/phpmyadmin/config.inc.php
```

Edit the configuration file:

```bash
vim /var/www/html/phpmyadmin/config.inc.php
```
Genrate a random secret passphrase to enhance security:

```bash
pwgen 32 -1
```
```bash
daemi9echiek6naSie2joa3eexiw5qua
```
Set proper ownersip to ensure Apache can access the phpMyAdmin files:

```bash
chown -Rv apache:apache /var/www/html/phpmyadmin
```
Restart Apache to apply change:
```bash
systemctl restart httpd.service
```
---
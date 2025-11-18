
# 🗄️ Installing MySQL Server

## 📥 Download MySQL APT Config Package

### Install prerequisites:

```bash
apt install wget
```

### Download package:

> https://dev.mysql.com/downloads/

```bash
wget https://dev.mysql.com/get/mysql-apt-config_0.8.36-1_all.deb
```

### Install it:

```bash
apt install ./mysql-apt-config_0.8.36-1_all.deb
```
     

### Update packages:

```bash
apt update
```
---

## 🗄️ Install MySQL
- check and select on your OS version:`Debian GNU/Linux 13 (trixie)`

```bash
PRETTY_NAME="Debian GNU/Linux 13 (trixie)"
NAME="Debian GNU/Linux"
VERSION_ID="13"
VERSION="13 (trixie)"
VERSION_CODENAME=trixie
DEBIAN_VERSION_FULL=13.2
ID=debian
HOME_URL="https://www.debian.org/"
SUPPORT_URL="https://www.debian.org/support"
BUG_REPORT_URL="https://bugs.debian.org/"
```   
```bash
apt install mysql-community-server
```

## 🚀 Enable and Start MySQL

```bash
systemctl restart mysql.service
```

```bash
systemctl enable mysql.service
```

---

## 🔎 Verify MySQL is Running

```bash
netstat -nltup
```

Check specifically for port **3306**:

```bash
netstat -nltup | grep 3306
```

---

## 🔐 Run Secure Setup

```bash
mysql_secure_installation
```

---

## 🛡️ Access MySQL

```bash
mysql -u root -p
```

### Inside MySQL:

```sql
show databases;
```

### Optional: Create a Remote Root User

```sql
CREATE USER 'root'@'%' IDENTIFIED WITH caching_sha2_password BY 'your_password';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
```

---

## 🔐 Enabling SSL on Apache

### ✔️ Enable SSL module:

```bash
a2enmod ssl
```

### 🔄 Restart Apache:

```bash
systemctl restart apache2.service
```

### 🌐 Enable default SSL site:

```bash
a2ensite default-ssl
```

### ♻️ Reload Apache:

```bash
systemctl reload apache2
```

### 📡 Check services:

```bash
netstat -nltup
```

```bash
service apache2 reload
```

---

## 🛠️ Creating a Self-Signed SSL Certificate

### 📁 Create SSL directory:

```bash
mkdir /etc/apache2/ssl
```

### 🧾 Generate certificate:

```bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/apache2/ssl/armour.local.key -out /etc/apache2/ssl/armour.local.crt
```

## 🔎 Verify SSL Files

### 🔐 View SSL private key:

```bash
cat /etc/apache2/ssl/armour.local.key
```

### 📄 View SSL certificate:

```bash
cat /etc/apache2/ssl/armour.local.crt
```

---

## 📝 Update Hosts File

```bash
vim /etc/hosts
```

### 🧩 Example entry:

```text
192.168.1.28   armour.local   www.armour.local
```

---

## 🛠️ phpMyAdmin Installation

### ⬇️ Download and unzip:

```bash
wget https://files.phpmyadmin.net/phpMyAdmin/5.2.3/phpMyAdmin-5.2.3-all-languages.zip
```

```bash
unzip phpMyAdmin-5.2.3-all-languages.zip
```

### 📦 Move files:

```bash
mv -v phpMyAdmin-5.2.3-all-languages /var/www/html/phpmyadmin
```

### 🔧 Set ownership:

```bash
chown -Rv www-data:www-data /var/www/html/phpmyadmin
```

### ⚙️ Create phpMyAdmin Config File

```bash
cp -v /var/www/html/phpmyadmin/config.sample.inc.php /var/www/html/phpmyadmin/config.inc.php
```

```bash
chown -Rv www-data:www-data /var/www/html/phpmyadmin/config.inc.php
```

### 🔑 Generate a Blowfish Secret

```bash
pwgen 32 -1
```

### 📝 Edit phpMyAdmin Config

```bash
vim /var/www/html/phpmyadmin/config.inc.php
```

### ➕ Insert Secret:

```php
$cfg['blowfish_secret'] = 'obooj1weixe5beiVaiqu8iehu8theiXi';
```

---

## 🛰️ Remote MySQL Login

```bash
mysql -h 192.168.1.23 -u root -p
```

---


## 🔐 Create SSL Config for phpMyAdmin

```bash
vim /etc/apache2/sites-available/phpmyadmin-ssl.conf
```

### ➕ Insert:

```apache
<VirtualHost 192.168.1.100:8443>
    DocumentRoot /var/www/html/phpmyadmin/
    SSLEngine on
    SSLCertificateFile /etc/apache2/ssl/armour.local.crt
    SSLCertificateKeyFile /etc/apache2/ssl/armour.local.key
    <Directory /var/www/html/phpmyadmin/>
        Options FollowSymlinks
        AllowOverride All
        Order allow,deny
        allow from all
    </Directory>
    ErrorLog /var/log/apache2/phpmyadmin-error_log
    CustomLog /var/log/apache2/phpmyadmin-access_log common
</VirtualHost>
```
## 🚀 Enable the Site
```bash
a2ensite phpmyadmin-ssl.conf 
```
## Check enable Sites:
```bash
a2ensite
```
- Check Port Listening:
```bash
netstat -nltup
```
- Add Port `8443`
```bash
vim /etc/apache2/ports.conf
```
```apache
# If you just change the port or add more ports here, you will likely also
# have to change the VirtualHost statement in
# /etc/apache2/sites-enabled/000-default.conf

Listen 80

<IfModule ssl_module>
	Listen 443
	Listen 8443
</IfModule>

<IfModule mod_gnutls.c>
	Listen 443
	Listen 8443
</IfModule>
````

- Check firewall Status:
```bash
ufw status
```
- Allow HTTPS (port 443):
```bash
ufw allow https
```
- allow specfic port `8443` in firewall:
```bash
ufw allow 8443/tcp
```
---

## 🌐 Configure Apache Virtual Host for HTTPS

### 📁 Create web root:

```bash
mkdir -p /var/www/html/armour.local
```

### 📝 Create virtual host file:

```bash
vim /etc/apache2/sites-available/armour.local-ssl.conf
```

### 🔧 Insert configuration:

```apache
<VirtualHost 192.168.1.100:80>
    ServerAdmin webmaster@armour.local
    ServerName armour.local
    ServerAlias www.armour.local
    DocumentRoot /var/www/html/armour.local
    <Directory /var/www/html/armour.local>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/armour.local-error.log
    CustomLog ${APACHE_LOG_DIR}/armour.local-access.log combined
</VirtualHost>

<VirtualHost 192.168.1.100:443>
    ServerAdmin webmaster@armour.local
    ServerName armour.local
    ServerAlias www.armour.local
    DocumentRoot /var/www/html/armour.local
    SSLEngine on
    SSLCertificateFile /etc/apache2/ssl/armour.local.crt
    SSLCertificateKeyFile /etc/apache2/ssl/armour.local.key
    <Directory /var/www/html/armour.local>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/armour.local-error.log
    CustomLog ${APACHE_LOG_DIR}/armour.local-access.log combined
</VirtualHost>
```



## 🚀 Enable the Site

```bash
a2ensite armour.local-ssl.conf
```

---

## ❌ Disable Default Sites

```bash
a2dissite 000-default.conf
```
```bash
a2dissite default-ssl.conf
```

---

## 🔄 Reload and Restart Apache

```bash
systemctl reload apache2
```
```bash
systemctl restart apache2.service
```

---

## 📥 Optional: Provide Certificate for Download

```bash
cp -v /etc/apache2/ssl/armour.local.crt /var/www/html
```

### 🌐 Access via browser:

```
http://192.168.1.45/armour.local.crt
```

---

## ⭐ Install WordPress

### ⬇️ Download and unzip:

```bash
wget https://wordpress.org/latest.zip
```

```bash
unzip latest.zip
```

### 📦 Move WordPress files:

```bash
mv -v wordpress/* /var/www/html/armour.local
```

### 🔧 Set permissions:

```bash
chown -Rv www-data:www-data /var/www/html/armour.local
```

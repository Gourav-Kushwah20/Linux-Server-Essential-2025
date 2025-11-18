# 🏗️ Apache2 Setup on Debian

This comprehensive guide walks you through installing and configuring **Apache2, PHP, MySQL, SSL, phpMyAdmin, and WordPress** on a Debian-based system. Ideal for setting up a local or production web server.

---

## 🔄 System Update

✔️ Ensure your system is up-to-date before beginning.

```bash
apt update
```

```bash
apt upgrade
```

---

## 🏛️ Installing Apache2

Install the Apache web server and networking tools:

```bash
apt install apache2
```
```bash
apt install apache2*
```

```bash
apt install net-tools
```

---

## 🔎 Verify Apache Installation

Check listening services and ensure Apache is running:

```bash
netstat -nltup
```

```bash
dpkg -l | grep apache
```

---


## ⚙️ Configure Apache

### ✏️ Edit Apache Config File

```bash
vim /etc/apache2/apache2.conf
```

### ✔️ Ensure these lines are included:

```apache
# Include generic snippets of statements
IncludeOptional conf-enabled/*.conf

# Include the virtual host configurations:
IncludeOptional sites-enabled/*.conf
```

---

## 🔄 Restart and Enable Apache Service

```bash
systemctl restart apache2.service
```

```bash
systemctl enable apache2.service
```


## 🔎 Confirm Apache is Listening

```bash
netstat -nltup
```

---

## 🚫 Disable Directory Listing

Improve security by removing the directory listing option:

```bash
sed -i "s/Options Indexes FollowSymLinks/Options FollowSymLinks/" /etc/apache2/apache2.conf
```

Restart Apache to apply changes:

```bash
systemctl restart apache2.service
```

---

## 🧩 Installing PHP

## 🔍 Search for PHP Versions

```bash
apt search php | grep "php/stable"
```

---

## 📦 Install PHP and Extensions

```bash
apt install php php8.4 php8.4-common php8.4-mbstring php8.4-xmlrpc php8.4-soap php8.4-gd php8.4-xml php8.4-intl php8.4-mysql php8.4-cli php8.4-ldap php8.4-zip php8.4-curl php-xml composer
```

---

## ⚙️ Configure PHP

### ✏️ Edit the configuration file:

```bash
vim /etc/php/8.4/apache2/php.ini
```

### ✅ Recommended settings:

```ini
memory_limit = 512M
max_execution_time = 500
max_input_vars = 10000
upload_max_filesize = 2048M
post_max_size = 2048M
allow_url_fopen = On
```
- Search using `/allow_url_fopen` operator and check all value of upper Settings:

![alt text](./img/image-6.png)

### 🔄 Restart Apache after configuration:

```bash
systemctl restart apache2.service
```

Here is the **markdown version with emojis**, based on your uploaded image:

---

## 🧪 Create a PHP Info Page

```bash
vim /var/www/html/phpinfo.php
```

### Insert:

```php
<?php
    phpinfo();
?>
```

### 📁 Set file ownership:

```bash
chown -Rv www-data:www-data /var/www/html/phpinfo.php
```

---

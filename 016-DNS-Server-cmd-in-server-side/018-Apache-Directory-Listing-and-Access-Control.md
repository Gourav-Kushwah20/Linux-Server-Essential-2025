# Apache Directory Listing & Access Control

### Change Directory and Create an Empty File
```bash
cd /var/www/html/wordpress/wp-content/uploads
````

```bash
touch index.php
```

---

### Disable Directory Listing for All Directories

#### 1. Edit the Apache Configuration File

```bash
vim /etc/httpd/conf/httpd.conf
```

```apache
<VirtualHost 192.168.1.35:443>
    SSLEngine on
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
    DocumentRoot /var/www/html/wordpress/
    DirectoryIndex index.php

    <Directory /var/www/html/wordpress>
        Options -Indexes
    </Directory>
</VirtualHost>
```

#### 2. Restart Apache

```bash
systemctl restart httpd.service
```

#### 3. (Optional) Edit Site-Specific Configuration

```bash
vim /etc/httpd/conf/httpd.conf
or 
vim /etc/httpd/conf.d/site1.conf
```

- Example:
```bash
<VirtualHost 192.168.1.23:80>
    DocumentRoot /var/www/html/site1/
    DirectoryIndex index.html
    <Directory /var/www/html/site1>
        Options -Indexes
    </Directory>
</VirtualHost>
```
#### Restart Apache

```bash
systemctl restart httpd.service
```
---

## Enable Directory Listing for a Selected Directory

### Create and Set Ownership
```bash
mkdir /var/www/html/wordpress/backup
````

```bash
chown -R apache:apache /var/www/html/wordpress/backup
```

---

### 2. Update Apache Configuration

```bash
vim /etc/httpd/conf/httpd.conf
```

```apache
<VirtualHost 192.168.1.35:443>
    SSLEngine on
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
    DocumentRoot /var/www/html/wordpress/
    DirectoryIndex index.php

    <Directory /var/www/html/wordpress/backup>
        Options Indexes
    </Directory>

    <Directory /var/www/html/wordpress>
        Options -Indexes
    </Directory>
</VirtualHost>
```

---

Restart Apache:

```bash
systemctl restart httpd.service
```

---

## 🌐 Allow Selected IP Addresses

```bash
vim /etc/httpd/conf/httpd.conf
```

```apache
<VirtualHost 192.168.1.35:443>
    SSLEngine on
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
    DocumentRoot /var/www/html/wordpress/
    DirectoryIndex index.php

    <Directory /var/www/html/wordpress/backup>
        Options Indexes
        Order allow,deny
        Allow from 192.168.1.7 192.168.1.51
    </Directory>

    <Directory /var/www/html/wordpress>
        Options -Indexes
    </Directory>
</VirtualHost>
```
Restart Apache:

```bash
systemctl restart httpd.service
```
---

## 🌐 Deny Selected IP Addresses


```bash
vim /etc/httpd/conf/httpd.conf
```

```apache
<VirtualHost 192.168.1.35:443>
    SSLEngine on
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
    DocumentRoot /var/www/html/wordpress/
    DirectoryIndex index.php

    <Directory /var/www/html/wordpress/backup>
        Options Indexes
        Order allow,deny
        Allow from all
        Deny from 192.168.1.51
    </Directory>

    <Directory /var/www/html/wordpress>
        Options -Indexes
    </Directory>
</VirtualHost>
```
Restart Apache:

```bash
systemctl restart httpd.service
```

---
# Secure Directory Hosting with User Authentication

## Using `.htaccess`

### 1. Edit `.htaccess` :

```bash
vim /var/www/html/wordpress/backup/.htaccess
````

```apache
AuthName "Armour Infosec"
AuthType Basic
AuthUserFile /etc/httpd/htpasswd
Require valid-user
```

---

### 2. Create Password File and Users:

```bash
htpasswd -c /etc/httpd/htpasswd user1
```
```bash
htpasswd /etc/httpd/htpasswd user2
```


**Example output:**

```text
user1:$apr1$e5izqEpL$hjMvtlTcuyGIKcRS2ISdt1
user2:$apr1$U1IwlefV$G7c78m4F2OH6gQeTEV7bF/
```
- Secure your password file:

```bash
chmod 640 /etc/httpd/htpasswd
```
```bash
chown root:apache /etc/httpd/htpasswd
```
```bash
cat /etc/httpd/htpasswd
```
---

### 3. Update Apache Configuration:

```bash
vim /etc/httpd/conf/httpd.conf
```

```apache
<VirtualHost 192.168.1.35:443>
    SSLEngine on
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
    DocumentRoot /var/www/html/wordpress/
    DirectoryIndex index.php

    <Directory /var/www/html/wordpress/backup>
        Options Indexes
        AllowOverride AuthConfig
    </Directory>

    <Directory /var/www/html/wordpress>
        Options -Indexes
    </Directory>
</VirtualHost>
```
Restart Apache:

```bash
systemctl restart httpd.service
```
---
## User Authentication Without `.htaccess`


### 1. Edit Apache Configuration:

```bash
vim /etc/httpd/conf/httpd.conf
````

```apache
<VirtualHost 192.168.1.35:443>
    SSLEngine on
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
    DocumentRoot /var/www/html/wordpress/
    DirectoryIndex index.php

    <Directory /var/www/html/wordpress/backup>
        AuthName "Private"
        AuthType Basic
        AuthBasicProvider file
        AuthUserFile /etc/httpd/htpasswd
        Require valid-user
        Options +Indexes
    </Directory>

    <Directory /var/www/html/wordpress>
        Options -Indexes
    </Directory>
</VirtualHost>
```
Restart Apache:

```bash
systemctl restart httpd.service
```
---

### 2. `If Not Create user` : Create Password File and Users:(Optional)

```bash
htpasswd -c /etc/httpd/htpasswd user1
```
```bash
htpasswd /etc/httpd/htpasswd user2
```
```bash
cat /etc/httpd/htpasswd
```

```bash
rm -rvf /var/www/html/site1/backup/.htaccess
```
Restart Apache:

```bash
systemctl restart httpd.service
```
---
## Verification Commands
- test access and Permissions:

```bash
curl -I http://armour.local/wordpress/uploads/
```
```bash
curl -I http://armour.local/wordpress/backup/
```
```bash
curl -I -u user1:123 http://armour.local/wordpress/backup/
```
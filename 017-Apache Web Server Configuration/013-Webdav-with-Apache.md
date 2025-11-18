# 🌐 WebDAV with Apache

## 🧩 Overview
**WebDAV** (Web Distributed Authoring and Versioning) extends the HTTP protocol to enable **remote file management** on a web server.  
Apache supports WebDAV using the [`mod_dav`](https://httpd.apache.org/docs/2.4/mod/mod_dav.html) and [`mod_dav_fs`](https://httpd.apache.org/docs/2.4/mod/mod_dav_fs.html) modules.

---

## ✨ Features of WebDAV
- 📁 Access and manage files over **HTTP/S**  
- ✏️ Supports **creation, editing, moving, and deletion** of files  
- 🔄 Integrates with version control tools like **Git** and **SVN**  
- 💻 Clients can **mount WebDAV directories as local drives**  
- 🤝 Suitable for **collaborative environments** and **remote file hosting**  

---

## ⚙️ 1. Install and Verify Apache

### 🔍 Check if Apache is Installed
```bash
rpm -qa | grep httpd
````

✅ Verifies whether Apache is already installed.

---

### 📦 Install Apache

```bash
yum install httpd*
```

⬇️ Installs Apache and related packages on **RHEL-based systems**.

---

### 🧰 List Enabled Apache Modules

```bash
httpd -M
```
📋 Lists all currently active Apache modules.

---
### Create for WebDAV Module Support
```bash
httpd -M | grep fs
```
Confirms that webDav support is enabled `mod_dav_fs`.

---


## 🔧 2. Create and Set Up WebDAV Directory

### 📂 Create Directory
```bash
mkdir /var/www/html/webdav
````

🪄 Sets up the directory for WebDAV content.

---

### 👑 Set Ownership

```bash
chown apache:apache /var/www/html/webdav/
```

✅ Ensures **Apache** owns the directory.

---

### 🔐 Set Permissions

```bash
chmod 777 /var/www/html/webdav/
```

⚠️ **For production:** Prefer using `755` or `775` for better security.

---
## 🧪 4. Configure Apache for WebDAV

- Edit Apache Configuration:

```bash
vim /etc/httpd/conf/httpd.conf
```

```apache
<VirtualHost *:80>
    DocumentRoot /var/www/html/
    DirectoryIndex index.html
    <Directory /var/www/html/webdav>
        DAV On
        Options Indexes
        AllowOverride None
        Require all granted
    </Directory>
</VirtualHost>
```

> If Upper Code will not work :`This code is working you can use that`

```bash
vim /etc/httpd/conf.d/webdav.conf
```
```apache
Alias /webdav /var/www/webdav
<Directory /var/www/webdav>
    DAV On
    Options Indexes
    AllowOverride None
    Require all granted
</Directory>
```

This enables **WebDAV** under the `/webdav` path. 🌐

---

## 🧪 **Test WebDAV Directory Access**

### 🔍 **Use curl to Send OPTIONS Request**

```bash
curl -v -X OPTIONS http://192.168.1.21/webdav/
```

✅ Verifies supported **HTTP methods** by the server.

---

## 🔥 **Configure SELinux and Firewall**

### ♻️ **Restart Apache**

```bash
systemctl restart httpd.service
```

### 🛡️ SELinux Configuration for WebDAV

---

### 🔍 **Check SELinux Status**

```bash
sestatus
```

---

### ⚠️ Disable SELinux Temporarily *(Not recommended for production)*

```bash
vim /etc/sysconfig/selinux
```

### 🔧 Change the value:

```bash
SELINUX=disabled
```

---
### ✅ Preferred: Use Proper SELinux Context Labeling

```bash
chcon -R -t httpd_sys_content_t /var/www/html/webdav
```

This keeps SELinux *enabled and secure* while allowing Apache to access WebDAV directories.

---

## 📤 Uploading and 📥 Deleting Files Using `curl`

- You can Perform this command in other machine like you main Machine:

### 📝 Upload a Text File

```bash
curl -X PUT -d "hi" http://192.168.1.21/webdav/1.txt
```

---

### 🐘 Upload a PHP File

```bash
curl -X PUT -d "<?php phpinfo(); ?>" http://192.168.1.21/webdav/phpinfo.php
```

---

## 🗑️ Delete Files

### Delete PHP file:

```bash
curl -X DELETE http://192.168.1.21/webdav/phpinfo.php
```

### Delete text file with verbose output:

```bash
curl -v -X DELETE http://192.168.1.21/webdav/1.txt
```

---

## 🔐 Secure WebDAV with Basic Authentication

- 👤 Create User Password File

```bash
htpasswd -c /etc/httpd/htpasswd dev
```
 
```bash
cat /etc/httpd/htpasswd
```
**dev:$apr1$2jrWoBh8$pc2V6KXp3vg8IPcIIBHy8/**

- ➕ Add More Users

```bash
htpasswd /etc/httpd/htpasswd admin
```
```bash
htpasswd /etc/httpd/htpasswd root
```
```bash
htpasswd /etc/httpd/htpasswd user
```

- 📄 View `.htpasswd` File

```bash
cat /etc/httpd/htpasswd
```
- 📌 Sample Output:

```
dev:$apr1$GxmFNd4f$HBa..Fu2SB1SMUncBcATuG/
admin:$apr1$nNmcN0p5$lHk2I/HVjXV7mKKpXis4C/
root:$apr1$Qey..Fqn$y2GIMsuP1JDDMnrwHgDnU.
user:$apr1$zp/J6/mB$v.h.deWHBq/34CABimwk3/
```

---

## 🛠️  7. Configure Apache for WebDAV Authentication

### ✏️ Edit Apache Config Again

```bash
vim /etc/httpd/conf/httpd.conf
```

---

### 🏗️ Update VirtualHost block:
```apache
<VirtualHost *:80>
    DocumentRoot /var/www/html/
    DirectoryIndex index.html
    <Directory /var/www/html/webdav>
        DAV On
        Options Indexes
        AllowOverride None
        Require all granted
    </Directory>
</VirtualHost>
```
> If upper code is `not working` this you can use below code`(Optional)`
```apache
<VirtualHost *:80>
    DocumentRoot /var/www/html/
    DirectoryIndex index.html
    <Directory /var/www/html/webdav>
        DAV On
        Options Indexes
        AllowOverride None
        AuthType Basic
        AuthName "webdav"
        AuthUserFile /etc/httpd/htpasswd
        Require valid-user
    </Directory>
</VirtualHost>
```
---

### 🔄 Restart Apache After Config Change

```bash
systemctl restart httpd.service
```

---

## 🛡️ Notes for Production Environments

* 🔒 **Always use HTTPS** to secure credentials in transit.
* 🛡️ Use **SELinux context labeling** instead of disabling it.
* 🚧 Apply **firewall rules** or **IP whitelisting** for added security.

```bash
chcon -R -t httpd_sys_content_t /var/www/html/webdav
```
Here is the **markdown version with emojis**, based on the content in your image:

---

# 🧪 WebDAV Client Testing


### 🔍 Test HTTP Methods

```bash
curl -v -X OPTIONS http://192.168.1.21/webdav/
```

```bash
curl -i -k -X OPTIONS https://192.168.1.21/webdav/
```

---

### 🛰️ Scan WebDAV with Nmap

```bash
nmap -v -sT -sV -A -O -p 80 --script=http-methods.nse --script-args http-methods.url-path='/webdav/' 192.168.1.21
```

---

# 🔐 Authenticated File Operations

---

### 🔑 OPTIONS with Auth

```bash
curl -v -u dev:123 -X OPTIONS http://192.168.1.21/webdav/
```

---

### 📤 Authenticated File Upload

```bash
curl -u dev:123 -X PUT -d "hello" http://192.168.1.21/webdav/1.txt
```

```bash
curl -X PUT -u dev:123 -d "<?php phpinfo(); ?>" http://192.168.1.21/webdav/phpinfo.php
```

---

### 🗑️ Authenticated File Deletion

```bash
curl -u dev:123 -X DELETE http://192.168.1.21/webdav/phpinfo.php
```

```bash
curl -v -u root:123 -X DELETE -d "hi" http://192.168.1.21/webdav/1.txt
```

---

# 🖥️ Using **cadaver** – WebDAV Command-Line Client

---

## 📦 Install cadaver

```bash
apt install cadaver
```

**or**

```bash
yum install cadaver
```

---

## 🔗 Connect to WebDAV Server

```bash
cadaver http://192.168.1.32/webdav/
```

---

## 🧰 Sample cadaver Session

```bash
dav:/webdav/> ?
```

```bash
dav:/webdav/> ls
```
```bash
dav:/webdav/> put network.png
```
```bash
dav:/webdav/> mkdir newdir
```
```bash
dav:/webdav/> mput *.php
```
```bash
dav:/webdav/> delete *
```

---

## 📁 Connect to a Different Path

```bash
cadaver http://192.168.1.32/test/
```

---

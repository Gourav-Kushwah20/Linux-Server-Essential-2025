# 🏠 Enable Apache User Home Directories

## 🌐 Overview

Apache allows individual users to host web content from their home directories using the **mod_userdir** module.  
This setup is ideal for 🧑‍💻 developers, 👨‍🏫 learners, or environments requiring isolated user web spaces.

---

## 🛠️ 1. Enable UserDir in Apache Configuration

### ✏️ Edit the `userdir.conf` File

```bash
vim /etc/httpd/conf.d/userdir.conf
````

### ⚙️ Sample Configuration

```apache
<IfModule mod_userdir.c>
    #
    # UserDir is disabled by default since it can confirm the presence
    # of a username on the system (depending on home directory permissions).
    #
    #UserDir disabled
    UserDir enabled
    #
    # To enable requests to /~user/ to serve the user's public_html
    # directory, remove the "UserDir disabled" line above, and uncomment
    # the following line instead:
    #
    UserDir public_html
</IfModule>

#
# Control access to UserDir directories.
# The following is an example for a site where these directories are restricted to read-only.
#
<Directory "/home/*/public_html">
    AllowOverride FileInfo AuthConfig Limit Indexes
    Options MultiViews Indexes SymLinksIfOwnerMatch IncludesNoExec
    Require method GET POST OPTIONS
</Directory>
```

### 🧩 This configuration:

* ✅ Enables `public_html` inside each user's home directory.
* ⚙️ Allows indexing, symbolic links, and `.htaccess` overrides.
* 🧱 Restricts access to safe HTTP methods only.

---

## 👤 2. Configure Home Directory for User: `sec-learner`

### 📂 Create Public Web Directory

```bash
mkdir /home/sec-learner/public_html
```

### 🔐 Set Correct Permissions

```bash
chmod 711 /home/sec-learner
chown -R sec-learner:sec-learner /home/sec-learner/public_html
chmod -R 755 /home/sec-learner/public_html
```

> 💡 **Note:**
> `711` on the home folder allows Apache to enter it, while `755` on `public_html` allows access to its content.

---

## 🌍 3. Create VirtualHost for `sec-learner`

### ✏️ Edit the VirtualHost File

```bash
vim /etc/httpd/conf.d/sec-learner.conf
```

### 🧾 Example VirtualHost Configuration

```apache
<VirtualHost 192.168.1.22:80>
    DocumentRoot /home/sec-learner/public_html
    DirectoryIndex index.html
</VirtualHost>
```

🔗 This binds the site to IP **192.168.1.22** and serves content from the user’s `public_html` directory.

---

## 👥 4. Configure Home Directory for Another User: `infosec`

### 🧑‍💻 Create User

```bash
useradd infosec
passwd infosec
```

### 📂 Create Public Web Directory

```bash
mkdir /home/infosec/public_html
```

### 🔐 Set Correct Permissions

```bash
chmod 711 /home/infosec/
```
```bash
chown -R infosec:infosec /home/infosec/public_html/
```

```bash
chmod -R 755 /home/infosec/public_html/
```

---

## 🌐 5. Create VirtualHost for `infosec`

### ✏️ Edit the VirtualHost File

```bash
vim /etc/httpd/conf.d/infosec.conf
```

### 🧾 Example: HTTP VirtualHost

```apache
<VirtualHost 192.168.1.23:80>
    DocumentRoot /home/infosec/public_html
    DirectoryIndex index.html
</VirtualHost>
```

### 🔒 Example: HTTPS VirtualHost

```apache
<VirtualHost 192.168.1.23:443>
    SSLEngine on
    SSLCertificateFile /etc/pki/tls/certs/ca.crt
    SSLCertificateKeyFile /etc/pki/tls/private/ca.key
    ServerName armour.com
    DocumentRoot /home/infosec/public_html
    DirectoryIndex index.html
    ServerAlias www.armour.com
    <Directory /home/infosec/public_html/>
        Options -Indexes
    </Directory>
</VirtualHost>
```

💡 The HTTP VirtualHost serves `infosec.com`, while the HTTPS VirtualHost uses SSL to serve `armour.com`.
Both can be customized for different domains.

---

## 🔄 6. Restart Apache to Apply Changes

```bash
systemctl restart httpd.service
```

---

## 📁 7. Copy Website Files

Example directory structure:

```bash
/Downloads]
└─# ls
site1  site2  site3  site4  site5  site6
```

📂 **Copy Site 5 to `sec-learner`**

```bash
cp -vr site5/* /home/sec-learner/public_html
```

📂 **Copy Site 6 to `infosec`**

```bash
cp -vr site6/* /home/infosec/public_html
```

🧹 **Clean Older Apache Config Entries**

```bash
vim /etc/httpd/conf/httpd.conf
```

---

🎉 **Setup Complete!**
You’ve successfully configured **Apache User Home Directories** for multiple users (`sec-learner` and `infosec`) with both **HTTP** and **HTTPS** access.
Now each user can host their own web projects securely and independently. 🚀


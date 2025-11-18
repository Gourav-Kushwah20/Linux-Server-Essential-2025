# 💻 CGI Scripts (Common Gateway Interface)

**CGI (Common Gateway Interface)** is a standard protocol used to run external programs or scripts on a web server to generate **dynamic web content**.  
These scripts can be written in various languages like 🐍 **Python**, 🐪 **Perl**, 🐚 **Shell**, or 🐘 **PHP**, and are commonly used to deliver content based on **user input**.

---

## ⚙️ How CGI Works

1. **🌐 Client Request:**  
   A user accesses a CGI-enabled URL by clicking a link or submitting a form.  

2. **🖥️ Server Execution:**  
   The web server runs the CGI script located in the `cgi-bin` directory.  

3. **📄 Dynamic Response:**  
   The script processes input and sends a dynamically generated HTML response to the client.

---

## 🧠 Supported Technologies

CGI scripts can be written in several programming languages:

- 🐪 **Perl** — Historically common for web scripting.  
- 🐍 **Python** — Readable and easy to maintain.  
- 🐘 **PHP** — Widely used for web development (though not strictly CGI).  
- 💎 **Ruby** — Flexible and dynamic.  
- 🐚 **Shell Scripts** — Useful for small utilities and server-side tasks.

---

# ⚙️ Installation and Configuration Steps

### 📦 Step 1: Install Apache and Perl CGI Module

```bash
yum install httpd*
````

```bash
yum install perl-CGI perl
```

```bash
yum groups install "Development Tools"
```
---
### ✅ Verify CGI Module is Enabled in Apache

Run the following commands to ensure the CGI module is active:  

```bash
httpd -M
````

```bash
httpd -M | grep cgi
```

---

### 🔐 Check SELinux Status

Ensure SELinux isn’t blocking CGI execution:

```bash
sestatus
```

---

# 📝 Edit Apache Configuration

### ⚙️ Edit the main Apache config file:

```bash
vim /etc/httpd/conf/httpd.conf
```

Example configuration block 👇

```apache
#
# "/var/www/cgi-bin" should be changed to whatever your ScriptAliased
# CGI directory exists, if you have that configured.
#
<Directory "/var/www/cgi-bin">
    AllowOverride None
    Options +ExecCGI
    AddHandler cgi-script .cgi .pl .py .sh
    Require all granted
</Directory>
```

---

# 🚀 Creating and Deploying CGI Scripts

### 🐪 Create a Simple CGI Script (Perl)

```bash
vim /var/www/cgi-bin/test.cgi
```

```perl
#!/usr/bin/perl
print "Content-type: text/html\n\n";
print "<h1>Server Memory Usage</h1>";
print "<pre>";
exec("free -h");
print "</pre>";
```

---

### 🧾 Set Ownership and Permissions

Assign correct ownership 👇

```bash
chown apache:apache /var/www/cgi-bin/test.cgi
```

Give execute permission 🔑

```bash
chmod 755 /var/www/cgi-bin/test.cgi
```

---

### 🔄 Restart Apache

Apply configuration changes:

```bash
systemctl restart httpd.service
```

---

### 🌐 Access the Script

Test your CGI script in a browser:

```bash
http://192.168.1.21/cgi-bin/test.cgi
```

🎉 If everything is configured correctly, you’ll see your **Server Memory Usage** output displayed dynamically!

---

# 🧩 Create Additional CGI Scripts

---

## 🧠 mem.cgi (Shell Script for Memory Usage)

📝 **Create the script:**
```bash
vim /var/www/cgi-bin/mem.cgi
````

💻 **Script content:**

```bash
#!/bin/bash
echo
echo "<h1>Server Memory Usage</h1>"
echo "<pre>"
free -h
echo "</pre>"
```

🔐 **Set permissions:**

```bash
chown apache:apache /var/www/cgi-bin/mem.cgi
chmod 755 /var/www/cgi-bin/mem.cgi
```

🌐 **Access the script:**

```
http://192.168.1.21/cgi-bin/mem.cgi
```

---

## 🌍 ping.cgi (Shell Script for Ping Test)

📝 **Create the script:**

```bash
vim /var/www/cgi-bin/ping.cgi
```

💻 **Script content:**

```bash
#!/bin/bash
echo
echo "Ping the Server"
ping -c 4 8.8.8.8
```

🔐 **Set permissions:**

```bash
chmod 755 /var/www/cgi-bin/ping.cgi
chown apache:apache /var/www/cgi-bin/ping.cgi
ls -lh /var/www/cgi-bin/ping.cgi
```

🌐 **Access the script:**

```
http://192.168.1.21/cgi-bin/ping.cgi
```

---

## 👋 hello.cgi (Perl Script for Hello World)

📝 **Create the script:**

```bash
vim /var/www/cgi-bin/hello.cgi
```

💻 **Script content:**

```perl
#!/usr/bin/perl
print "Content-type: text/html\n\n";
print "<html><head><title>CGI Script</title></head><body>";
print "<h1>Hello, World!</h1>";
print "</body></html>";
```

🔐 **Set permissions:**

```bash
chmod +x /var/www/cgi-bin/hello.cgi
```

📁 **Verify file:**

```bash
ls -lh /var/www/cgi-bin/hello.cgi
```

🌐 **Access:**

```
http://192.168.1.21/cgi-bin/hello.cgi
```

---

## 🐍 hello2.cgi (Python3 Script for Hello World)

📝 **Create the script:**

```bash
vim /var/www/cgi-bin/hello2.cgi
```

💻 **Script content:**

```python
#!/usr/bin/python3
print("Content-type: text/html\n\n")
print("<html><head><title>CGI Script</title></head><body>")
print("<h1>Hello, CGI World!</h1>")
print("</body></html>")
```

🔐 **Set permissions:**

```bash
chmod +x /var/www/cgi-bin/hello2.cgi
chown apache:apache /var/www/cgi-bin/hello2.cgi
ls -lh /var/www/cgi-bin/hello2.cgi
```

🌐 **Access:**

```
http://192.168.1.21/cgi-bin/hello2.cgi
```

---

# 🛡️ Securing the cgi-bin Directory

## 🔒 Restrict Access to Authenticated Users

🧩 **Edit Apache config:**

```bash
vim /etc/httpd/conf/httpd.conf
```

💻 **Example secure block:**

```apache
#
# "/var/www/cgi-bin" should be changed to whatever your ScriptAliased
# CGI directory exists, if you have that configured.
#
<Directory "/var/www/cgi-bin">
    AllowOverride None
    Options +ExecCGI
    AddHandler cgi-script .cgi .pl .py .sh
    AuthType Basic
    AuthName "Armour CGI"
    AuthUserFile /etc/httpd/htpasswd
    Require valid-user
</Directory>
```

🔄 **Restart Apache:**

```bash
systemctl restart httpd.service
```

🌐 **Access the script:**

```
http://192.168.1.21/cgi-bin/test.cgi
```

---

> ✅ **Summary:**
You’ve now created multiple CGI scripts in different languages (Shell 🐚, Perl 🐪, Python 🐍), configured Apache for CGI execution, and secured access with authentication.
Your server is now ready to handle dynamic content securely and efficiently! 🚀



# 🔐 User Authentication

## 🟩 Enable Basic NCSA Authentication in Squid

## 🧰 Install Required Tools

Install the **httpd-tools** package, which provides the `htpasswd` command:

```bash
yum install httpd-tools -y
```

---

## 👤 Create the Password File and Users

### ➕ Create the password file and the first user:

```bash
htpasswd -c /etc/squid/passwd u1
```

### ➕ Add additional users (without the `-c` flag):

```bash
htpasswd /etc/squid/passwd u2
```

---

## 📄 Confirm the file:

```bash
cat /etc/squid/passwd
```

---

## 🔒 Set correct permissions:

```bash
ls -lh /etc/squid/passwd
```

```bash
chgrp squid /etc/squid/passwd
```

```bash
chmod 640 /etc/squid/passwd
```

```bash
ls -lh /etc/squid/passwd
```

---
## 🔧 Update `squid.conf` for Authentication

---

## ✏️ Edit your Squid configuration file:

```bash
vim /etc/squid/squid.conf
```

---

## ➕ Add the following lines

(Near the top or inside the section marked for custom rules):

```bash
auth_param basic program /usr/lib64/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic realm Squid Proxy Authentication
acl ncsa_users proxy_auth REQUIRED
http_access allow ncsa_users
```

---

## 🔐 Optional: Restrict access to specific users

```bash
acl allowed_users proxy_auth u1 u2
http_access allow allowed_users
```

⚠️ **Important:**
Ensure the `http_access allow` line appears **before** any general:

```bash
http_access deny all
```

Otherwise, it will be ignored.

---

## 🔁 Restart Squid

Apply the configuration:

```bash
systemctl restart squid
```

---

## 🌐 Test the Proxy

Try accessing any website via Squid in a browser — it should prompt you for a**username & password**.
Only users listed in:

```
/etc/squid/passwd
```

will be able to connect.

---

## 📝 Notes & Tips

### 📌 Always back up your `squid.conf` before making changes.

### 👤 Manage users later with `htpasswd`:

* **Add user:**  `htpasswd /etc/squid/passwd newuser`


* **Delete user:**
  Manually open the file and remove the line.

---

- Check Logs
```bash
tail -f /var/log/squid/access.log
```
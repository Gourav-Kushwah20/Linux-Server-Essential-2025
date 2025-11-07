# 🛠️ Linux usermod Command Examples

### 📌 Show help
```bash
usermod --help
```
### 🔎 Check user `u19` in `/etc/passwd`
```bash
cat /etc/passwd | grep u19
Output:
u19:x:1022:1010::/home/u19:/bin/bash
```
### 📝 Add Comment (Description) to User
```bash
usermod -c "HR user" u19
```

```bash
cat /etc/passwd | grep u19
Output:
u19:x:1022:1010:HR user:/home/u19:/bin/bash
```
### 🖥️ Change Login Shell

```bash
usermod -s /bin/sh u19
```
```bash
usermod -s /sbin/nologin u19
```
### 🔑 Change User ID (UID)
```bash
usermod -u 1050 u19
```
### 👥 Change Primary Group

```bash
usermod -g 1027 u19
```
### 📂 Change Home Directory
```bash
usermod -d /data/u19 u19
```

```bash
cat /etc/passwd | grep u19
```

### 🔐 Generate Password Hash
```bash
openssl passwd 123
Output:
$1$S/Y1ME6P$sSU5Ft0aqN9VNdYuC8394/
```

### 📝 Update User Password (`/etc/shadow`)
```bash
cat /etc/passwd /etc/shadow | grep u19
```

```bash
usermod -p '$1$S/Y1ME6P$sSU5Ft0aqN9VNdYuC8394/' u19
```

```bash
cat /etc/passwd /etc/shadow | grep u19
```

### ⚠️ No Password for u3 (Can't Login)

### 👉 Set password:

```bash
usermod -p '$1$S/Y1ME6P$sSU5Ft0aqN9VNdYuC8394/' u3
```
### 🔒 Lock / 🔓 Unlock User

```bash
usermod -L u3
```

```bash
usermod -U u3
```
### ⏳ Set Account Expiry
```bash
usermod -e 2020-12-30 u18
```
### 🕒 Set Inactive Days After Password Expiry
```bash
usermod -f 8 u18
```


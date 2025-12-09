# 🔓 Enable-Root-Login-via-SSH

Allowing root login can be **risky** and is usually discouraged in production environments. Only do this in secure, trusted, or isolated environments (like labs or VMs).

---

## 📝 Edit the Configuration

## 🟦 Option 1: Main Config File

### Edit the main SSH config:

```bash
vim /etc/ssh/sshd_config
```

Ensure this line exists and **is not commented**:

```bash
PermitRootLogin yes
```

---

## 🟩 Option 2: Drop-in Config

Alternatively, create or edit the drop-in config:

```bash
vim /etc/ssh/sshd_config.d/01-permitrootlogin.conf
```

Set:

```bash
PermitRootLogin yes
```

> 📝 This will override any conflicting setting from the main config.

---

## 🔍 Step 2: Check for Conflicts

Make sure no other file still sets it to **no**:

```bash
grep -r "PermitRootLogin" /etc/ssh/sshd_config*
```

Comment or remove any lines like:

```bash
PermitRootLogin no
```
---
Here is the **Markdown version with emojis**, matching your screenshot:

---

## 🔄 Restart SSH

### Apply changes:

```bash
systemctl restart sshd.service
```

---

## 🧪 Test Root Login

Try logging in as root (use your custom port if configured):

```bash
ssh -p 2200 root@192.168.1.32
```

> If successful, the shell should drop you into a root session.

---

# ⚠️ Security Notes

### 🔸 If `PasswordAuthentication no` is enabled

Root login will **still fail** unless you use key-based authentication.

---

### 🔸 On SELinux systems

Make sure the port is allowed for SSH:

```bash
semanage port -l | grep ssh
```

---

### 🔸 If using firewalld

Make sure your port (e.g., **2200**) is open:

```bash
firewall-cmd --permanent --add-port=2200/tcp
firewall-cmd --reload
```

---

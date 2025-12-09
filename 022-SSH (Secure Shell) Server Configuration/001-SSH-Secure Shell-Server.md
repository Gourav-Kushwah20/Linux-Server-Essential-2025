# 🔐 SSH (Secure Shell) Server


## 📝 Check If OpenSSH Is Already Installed

### ✔️ To verify if any SSH-related packages are already installed:

```bash
rpm -qa | grep ssh
```

---

## 🗑️ Remove Existing OpenSSH Server (If Needed)

### If an existing OpenSSH server installation needs to be removed before installing a fresh one:

```bash
yum remove openssh-server
```

---

## 📦 Install OpenSSH Packages

### 🟦 Install all SSH-related packages using a wildcard:

```bash
yum install openssh*
```

### Alternatively, install them individually for more control:

```bash
yum install openssh-server
```

### Install the client tools for connecting to SSH servers:

```bash
yum install openssh-clients
```

---

## 🔍 Check Package Information


### 📦 Retrieve detailed information about the installed OpenSSH server package:

```bash
rpm -qi openssh-server
```

---

### 📂 List all the files installed by the OpenSSH server package:

```bash
rpm -ql openssh-server
```

---

### ⚙️ List all configuration files related to the OpenSSH server:

```bash
rpm -qc openssh-server
```

---

### 📚 Display documentation files included in the OpenSSH server package:

```bash
rpm -qd openssh-server
```

---

## 🛠️ Manage the SSH Daemon with systemd

### 🔍 Check the current status of the SSH daemon:

```bash
systemctl status sshd.service
```

---

### ▶️ Start the SSH service:

```bash
systemctl start sshd.service
```

---

### 🔁 Enable SSH to start automatically on boot:

```bash
systemctl enable sshd.service
```

---

## ✅ Verify SSH Service Is Running

### Check if the SSH daemon is listening on the expected ports (usually port 22):

```bash
netstat -nltup | grep sshd
```


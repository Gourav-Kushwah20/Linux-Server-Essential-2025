# 🖥️ SSH Client Tools

SSH (Secure Shell) is a cryptographic network protocol that allows secure access to remote systems over unsecured networks.
It encrypts the entire session, including passwords and commands.

---

## 🟦 Basic SSH Usage

### ▶️ Connect to a remote system using the default username and port 22:

```bash
ssh 192.168.1.33
```

---

### 🔑 Use the `-l` flag or direct format to log in as the **root** user:

```bash
ssh -l root 192.168.1.33
```

```bash
ssh root@192.168.1.33
```

---

### 🚪 Connect using a non-default port (when SSH server listens on another port):

```bash
ssh -l root -p 2222 192.168.1.33
```

```bash
ssh -p 2200 root@192.168.1.33
```

---

## 🔐 Use Identity File for Authentication

Specify a private key file to authenticate and bypass password prompts:

```bash
ssh -i id_rsa_centos root@192.168.1.33
```

```bash
ssh -i ~/.ssh/id_ecdsa root@192.168.1.33
```
```bash
ssh -p 2200 -i ~/.ssh/id_ecdsa root@192.168.1.33
```
---

## 🐞 Enable Verbose Output for Debugging

Get detailed connection output to troubleshoot issues:

```bash
ssh -v root@192.168.1.33
```

```bash
ssh -vvv -i ~/.ssh/id_rsa root@192.168.1.33
```
---

## ▶️ Execute One-Line Commands Over SSH

Run simple commands on a remote machine **without interactive login**:

```bash
ssh root@192.168.1.33 id
```

```bash
ssh root@192.168.1.33 -p 22 id
```

```bash
ssh root@192.168.1.33 'uptime'
```

```bash
ssh root@192.168.1.33 -p 22 -i .ssh/id_rsa 'cat /etc/hostname'
```

```bash
ssh -i ~/.ssh/id_rsa root@192.168.1.33 "df -h"
```

---

## 🔵 Local Port Forwarding

Forward a **local port** to a port on the remote server:

```bash
ssh -L 8080:localhost:80 root@192.168.1.33
```

---

## 🟣 Remote Port Forwarding

Forward a **remote port** to a port on the local system:

```bash
ssh -R 9090:localhost:80 root@192.168.1.33
```

---

## 🛡️ Use Jump Host (SSH Proxying)

Connect through an intermediary jump host (gateway) to reach a private/internal system.
```bash
ssh -J root@gateway root@192.168.1.33
```
---

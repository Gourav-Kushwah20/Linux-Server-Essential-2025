# 📡 Trivial-File-Transfer-Protocol (TFTP) Server

## 🔧 Trivial File Transfer Protocol (TFTP) Setup on RHEL/CentOS

---

## 📦 Package Verification and Installation

### ✅ Check if the **tftp** client or server is already installed:

```bash
rpm -qa | grep tftp
```

### 📥 Install both the TFTP client and server:

```bash
yum install tftp tftp-server
```

---

## 🧾 TFTP Package Information

### 🔍 View details about the installed TFTP server package:

```bash
rpm -qi tftp-server
```

### 📂 List all files installed by the **tftp-server** package:

```bash
rpm -ql tftp-server
```

### ⚙️ Display the configuration files:

```bash
rpm -qc tftp-server
```

### 📘 Show documentation files provided by the package:

```bash
rpm -qd tftp-server
```

---
## 📁 Directory Setup for TFTP Boot

### 📍 Navigate to the default TFTP root directory:

```bash
cd /var/lib/tftpboot/
```

### 🔍 Check if the directory exists:

```bash
ls -lha /var/lib/ | grep "tftpboot"
```

### ⚠️ Set full permissions on the directory

*(for testing only — **not recommended** for production)*:

```bash
chmod -R 777 /var/lib/tftpboot/
```

### 📤 Copy test files (like logs) into the TFTP root directory:

```bash
cp -v /var/log/* /var/lib/tftpboot/
```

---

## 🛠️ Systemd Configuration for TFTP

### 📄 Copy the default systemd service & socket unit files to the custom path:

```bash
cp -v /usr/lib/systemd/system/tftp.service /etc/systemd/system/tftp-server.service
```

```bash
cp -v /usr/lib/systemd/system/tftp.socket /etc/systemd/system/tftp-server.socket
```

### 📚 Review the TFTP daemon documentation:

```bash
man in.tftpd
```

---


## ⚙️ TFTP Server Service File (`tftp-server.service`)

```bash
vim /etc/systemd/system/tftp-server.service
```

```ini
[Unit]
Description=Tftp Server
Requires=tftp-server.socket
Documentation=man:in.tftpd

[Service]
ExecStart=/usr/sbin/in.tftpd -c -p -s /var/lib/tftpboot
StandardInput=socket

[Install]
WantedBy=multi-user.target
Also=tftp-server.socket
```

---

## 🔌 TFTP Server Socket File (`tftp-server.socket`)

```bash
vim /etc/systemd/system/tftp-server.socket
```

```ini
[Unit]
Description=Tftp Server Activation Socket

[Socket]
ListenDatagram=69
BindIPv6Only=both

[Install]
WantedBy=sockets.target
```
---
## 🚀 Starting and Enabling the TFTP Server

### 🔄 Restart the TFTP socket to activate the service:

```bash
systemctl restart tftp-server.socket
```

### ⚙️ Enable the service to start at boot:

```bash
systemctl enable tftp-server
```

---

## 🔥 Firewall Configuration Using iptables

### ✔️ Check if **firewalld** is active:

```bash
systemctl status firewalld
```

---

### ▶️ Start and enable **firewalld** if not running:

```bash
systemctl start firewalld
```

```bash
systemctl enable firewalld
```

---

### 🌐 Add the TFTP service to the **public zone** permanently:

```bash
firewall-cmd --zone=public --add-service=tftp --permanent
```

---

### 📡 Or open UDP port **69** directly:

```bash
firewall-cmd --zone=public --add-port=69/udp --permanent
```

---

### 🔄 Reload the firewall configuration:

```bash
firewall-cmd --reload
```

---

### 🧪 Verify the rule is applied:

```bash
firewall-cmd --zone=public --list-all
```
---


## 🔍 Verifying TFTP Server Status

Use **netstat** or **ss** to confirm that TFTP is listening on UDP port **69**:

### 📡 Check with `netstat`:

```bash
netstat -nltup | grep 69
```

### 📡 Or check with `ss`:

```bash
ss -uln | grep 69
```

### ✅ Expected output:

You should see **in.tftpd** listening on:

* `0.0.0.0:69`
* or `[::]:69`
---

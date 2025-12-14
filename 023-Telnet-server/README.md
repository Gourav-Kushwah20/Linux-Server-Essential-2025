# Telnet Server

## 📦 Install Telnet Packages

### ➤ First, check if Telnet is already installed:

```bash
rpm -qa | grep telnet
```

---

### ➤ If Telnet is not installed, install it along with required components:

```bash
yum install telnet*
```

---

### ➤ Install specific Telnet server and client packages:

```bash
yum install telnet-server telnet
```

---

### ➤ Install xinetd, which acts as a super-server for Telnet: (NOT INSTALLED)

```bash
yum install xinetd
```

---

### ➤ Verify installation:

```bash
rpm -qa | grep telnet
```

---

## 🔍 Inspect Installed Packages

### ➤ Get detailed information about the Telnet server:

```bash
rpm -qi telnet-server
```

---

### ➤ List all files provided by the Telnet server package:

```bash
rpm -ql telnet-server
```
---

### 📚 List all documentation files related to the Telnet server:

```bash
rpm -qd telnet-server
```

---

### 🗂️ List configuration files associated with the Telnet server:

```bash
rpm -qc telnet-server
```

---

### 📘 Similarly, check information about xinetd: (NOT Installed Showing error)

```bash
rpm -qi xinetd
```

---

### 📄 List xinetd files:

```bash
rpm -ql xinetd
```

---

### 📖 List xinetd documentation files:

```bash
rpm -qd xinetd
```

---

### ⚙️ List xinetd configuration files:

```bash
rpm -qc xinetd
```

---

## 🛠️ Configure Systemd (Native Method)

This method directly manages Telnet through systemd without relying on xinetd.

---

## 🧩 Create Telnet Socket Unit

Edit the Telnet socket unit file:

```bash
vim /etc/systemd/system/telnet.socket
```

### 📝 Insert the following configuration:

```ini
[Unit]
Description=Telnet Server Activation Socket

[Socket]
ListenStream=23
Accept=yes

[Install]
WantedBy=sockets.target
```

---

## 🧩 Create Telnet Service Unit

### ➤ Create the Telnet service file:

```bash
vim /etc/systemd/system/telnet@.service
```

### 📝 Insert the following content:

```ini
[Unit]
Description=Telnet Server Service

[Service]
ExecStart=-/usr/sbin/in.telnetd
StandardInput=socket
```

---

## 🚀 Enable And Start Telnet Socket

### ➤ Enable the socket at boot time:

```bash
systemctl enable telnet.socket
```

### 🔥 Start the socket immediately:

```bash
systemctl start telnet.socket
```

---

## 🛡️ Configure Firewall

To allow Telnet (port 23 TCP) through the firewall, follow these steps:

### 🔧 Add Port 23/TCP Permanently

This command adds TCP port 23 to the permanent firewall rules:

```bash
firewall-cmd --permanent --add-port=23/tcp
```

Expected output:

```bash
success
```

---

## 🔄 Reload Firewall Rules

Reload the firewall to apply the new settings:

```bash
firewall-cmd --reload
```

Expected output:

```bash
success
```

---

## 🔍 Verify Open Ports

List all open ports to confirm that port 23/TCP is active:

```bash
firewall-cmd --list-ports
```

### Example output:

```bash
23/tcp
```

If you see **23/tcp** in the output, the firewall rule has been successfully added.

---

## 🔍 Verify Service Status

### ✔️ Check if Telnet is listening on port 23:

```bash
netstat -nltup | grep 23
```

---

### ✔️ Test connection from a client machine:

```bash
telnet 192.168.1.21
```

---

## ⚠️ Enable Root Login Via Telnet (Optional And Risky)

By default, root login may be disabled over Telnet. To allow it, update the securetty file.

### 📝 Edit the securetty file:

```bash
vim /etc/securetty
```

### ➤ Add the following lines:

```
pts/0
pts/1
pts/2
pts/3
pts/4
pts/5
pts/6
pts/7
pts/8
pts/9
```

### 📝 **Note:**

Allowing root login over Telnet is highly discouraged unless necessary for testing in a controlled environment.

---

### ⚠️ **Important Security Warning**

* ⚠️ Telnet transmits data, including passwords, in plain text.
* 🔐 It is recommended to use SSH instead of Telnet for secure remote access.
* 🏠 If Telnet must be used, restrict its access to internal, trusted networks only.
* 🛡️ Implement additional security measures such as TCP Wrappers or IP address restrictions.

---


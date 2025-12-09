# 🖥️ Remote Desktop Setup

## Server Options

### 🔵 XRDP

* Xrdp is an open-source RDP (Remote Desktop Protocol) server.
* It allows remote connections from RDP clients such as Windows' built-in Remote Desktop Connection.

---

### 🟠 VNC

* VNC (Virtual Network Computing) enables graphical desktop sharing.
* Multiple server options are available, such as **tigervnc**, **tightvnc**, and **realvnc**.

---

### 🟣 Nomachine

* NoMachine is a high-performance remote desktop software.
* It is optimized for both local and WAN usage, offering a smoother experience compared to traditional VNC or RDP.

---

## 🖥️ Client Options

---

### 🟢 Rdesktop

* Rdesktop is a lightweight and simple RDP client for Linux.
* It connects to Windows systems or any server supporting RDP.

---

## 📦 Install EPEL Release Package

First, install the EPEL (Extra Packages for Enterprise Linux) repository:

```bash
yum -y install epel-release
```

---


## 📥 Install Nux Dextop Repository

Download and install the Nux Dextop repository which includes **rdesktop**:

```bash
rpm -Uvh http://li.nux.ro/download/nux/dextop/el7/x86_64/nux-dextop-release-0-5.el7.nux.noarch.rpm
```

---

## 💽 Install Rdesktop

After setting up the repositories, install rdesktop:

```bash
yum install rdesktop
```

---

## 🖥️ Basic Rdesktop Connection

To connect to a remote desktop using just an IP address:

```bash
rdesktop 192.168.1.61
```

---

## 🔑 Rdesktop Connection With Username And Password

To connect specifying a username and password:

```bash
rdesktop 192.168.1.61 -u rahul -p 123
```

---

## 🖥️ xfreerdp

**xfreerdp** is a modern and feature-rich Remote Desktop Protocol (RDP) client for Linux. It supports various RDP features and provides additional flexibility.

---

## 🟦 On CentOS/RHEL

To install **xfreerdp** on CentOS/RHEL, you need to enable the **EPEL** repository first, and then install **freerdp**:

```bash
yum install epel-release
```

```bash
yum install freerdp
```

---

## 🟩 On Ubuntu/Debian

To install **xfreerdp** on Ubuntu or Debian, use **APT**:

```bash
apt update
```

```bash
apt install freerdp2-x11
```

---

## 🟧 On Fedora

To install **xfreerdp** on Fedora, use **dnf**:

```bash
dnf install freerdp
```

---

## 🟨 On Arch Linux

On Arch Linux, you can install **xfreerdp** from the official repositories:

```bash
pacman -S freerdp
```

---

## 🧩 Basic Command Structure

The basic syntax for running **xfreerdp** is:

```bash
xfreerdp [options] /u:<username> /p:<password> /v:<server_address>
```

Example:

```bash
xfreerdp /u:rahul /p:123 /v:192.168.1.61
```

* `/u` = Username
* `/p` = Password
* `/v` = Server address (IP or hostname)

---

## 🎛️ Commonly Used Options

---

## 🖥️ Fullscreen Mode

To launch **xfreerdp** in fullscreen mode, use the **+f** option:

```bash
xfreerdp3 /u:clinet-1 /p:123 /v:192.168.1.71 +f
```

* **+f** : Forces xfreerdp to enter fullscreen mode.

---

## 📺 Set Screen Resolution

To set a specific screen resolution for the remote session, use the **/size** option:

```bash
xfreerdp /u:rahul /p:123 /v:192.168.1.61 +f /size:1920x1080
```

* `/size:1920x1080` : Sets the resolution to Full HD (1920×1080).
* You can adjust the resolution as needed, e.g., **1366×768**, **1280×720**, etc.

---

## 💾 Redirect Local Drives

To redirect your local drives to the remote session, use the **/drive** option:

```bash
xfreerdp /u:rahul /p:123 /v:192.168.1.61 +f /drive:/opt/share
```

* `/drive:<local_path>` : Redirects a local directory to the remote session.

    ---

## 📋 Redirect Local Clipboard

To enable clipboard sharing between local and remote systems:

```bash
xfreerdp /u:rahul /p:123 /v:192.168.1.61 +f +clipboard
```

* **+clipboard** : Enables clipboard redirection, allowing you to copy and paste between local and remote systems.

---

## 🔊 Redirect Local Audio

To redirect local audio to the remote session, use the **/audio** option:

```bash
xfreerdp /u:rahul /p:123 /v:192.168.1.61 +f +audio
```

* **+audio** : Redirects audio from the remote desktop to the local machine.

---

## 🖱️ Redirect USB Devices

To redirect USB devices (such as a USB mouse or USB storage), use the **/usb** option:

```bash
xfreerdp /u:rahul /p:123 /v:192.168.1.61 +f +usb
```

* **+usb** : Enables USB redirection.

---

## 🔐 Disable TLS Security

If you're connecting to a server that doesn’t support modern encryption, you can use the **/sec** option to change the security mode:

```bash
xfreerdp /u:rahul /p:123 /v:192.168.1.61 +f /sec:rdp
```

* **/sec:rdp** : Forces xfreerdp to use RDP (older, less secure) security mode instead of the default TLS.

---

## 🛡️ Use Network Level Authentication (NLA)

If the remote server requires **NLA** (Network Level Authentication), ensure it's enabled:

```bash
xfreerdp /u:rahul /p:123 /v:192.168.1.61 +f +tls
```

* **+tls** : Forces the use of **TLS** encryption for secure connections (often required by newer servers).

---

## ⚡ Quick Summary

| **Option**     | **Description**       | **Example**                                                                |
| -------------- | --------------------- | -------------------------------------------------------------------------- |
| **+f**         | Fullscreen mode       | `xfreerdp /u:rahul /p:123 /v:192.168.1.61 +f`                              |
| **/size:**     | Set screen resolution | `xfreerdp /u:rahul /p:123 /v:192.168.1.61 +f /size:1920x1080`              |
| **+clipboard** | Redirect clipboard    | `xfreerdp /u:rahul /p:123 /v:192.168.1.61 +f +clipboard`                   |
| **+audio**     | Redirect audio        | `xfreerdp /u:rahul /p:123 /v:192.168.1.61 +f +audio`                       |
| **/drive:**    | Redirect local drive  | `xfreerdp /u:rahul /p:123 /v:192.168.1.61 +f /drive:/home/rahul/Documents` |
| **/sec:rdp**   | Use RDP security      | `xfreerdp /u:rahul /p:123 /v:192.168.1.61 +f /sec:rdp`                     |
| **+usb**       | Redirect USB devices  | `xfreerdp /u:rahul /p:123 /v:192.168.1.61 +f +usb`                         |

---

## 🟦 Vncviewer

* VNCviewer is a client application used to connect to VNC servers.
* Different VNC viewers include **tigervnc-viewer**, **realvnc**, and **tightvnc**.

---

## 🧩 Install Vncviewer On CentOS/RHEL

Install the TigerVNC client:

```bash
yum install tigervnc
```

---

## 🧩 Install Vncviewer On Debian/Ubuntu

Install the VNC viewer on Ubuntu or Debian systems:

```bash
apt install tigervnc-viewer
```

---

## 🖥️ Basic Vncviewer Connection

Connect to a VNC server by specifying its IP address and port (default VNC port is **5901**):

```bash
vncviewer 192.168.1.61:5901
```
---

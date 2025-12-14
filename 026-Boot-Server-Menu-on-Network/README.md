# 🖥️ Preboot-eXecution-Environment (PXE) Boot Server

![alt text](image.png)

## ⚙️ PXE Boot Server Setup

## 📦 Install Necessary Packages

Install the required services for a PXE boot server:

```bash
yum install dhcp-server vsftpd tftp-server syslinux
```

> ℹ️ **Note:** No `xinetd` is needed. TFTP is managed by **systemd** now.

---

## 1️⃣ DHCP Server Configuration

### ✏️ Edit the DHCP configuration file:

```bash
vim /etc/dhcp/dhcpd.conf
```

### ➕ Add the following configuration:

```conf
authoritative;

subnet 192.168.1.0 netmask 255.255.255.0 {
    option routers 192.168.1.1;
    option domain-name-servers 8.8.8.8, 8.8.4.4;
    option broadcast-address 192.168.1.255;
    default-lease-time 60000;
    max-lease-time 720000;
    range 192.168.1.150 192.168.1.200;
    next-server 192.168.1.21;
    filename "pxelinux.0";
}
```

📌 **Explanation:**

* `authoritative` → This DHCP server is authoritative for the network
* `range` → IP pool for PXE clients
* `next-server` → PXE/TFTP server IP
* `filename` → PXE bootloader file

---

## 🔁 Enable and Restart DHCP Server

```bash
systemctl enable --now dhcpd
```

---

## 2️⃣ FTP Server Configuration

## ✏️ Edit FTP Configuration File

Open the **vsftpd** configuration file:

```bash
vim /etc/vsftpd/vsftpd.conf
```

---

## ⚙️ Set the Following Options

```conf
anonymous_enable=YES
local_enable=YES
write_enable=YES
listen=YES
listen_ipv6=NO
allow_writeable_chroot=YES
pam_service_name=vsftpd
pasv_enable=YES
pasv_min_port=55000
pasv_max_port=55999
```

📌 **Explanation:**

* Enables anonymous and local FTP access
* Allows file uploads and writes
* Configures passive FTP ports (required for PXE installs)

---

## 🔁 Enable and Restart FTP Server

```bash
systemctl enable --now vsftpd
```

---

## 3️⃣ Prepare Installation Files

## 📀 Mount CentOS 9 DVD (or ISO) and Copy Contents

```bash
mount /dev/sr0 /mnt/
```

```bash
mkdir -p /var/ftp/centos9/
```

```bash
cp -vr /mnt/* /var/ftp/centos9/
```

```bash
chmod -R 755 /var/ftp/centos9/
```

 📁 Copy Boot Images to TFTP Directory

Copy PXE boot images from the CentOS FTP directory to the TFTP root:

```bash
cp -vr /var/ftp/centos9/images/pxeboot /var/lib/tftpboot/images
```

---

## 4️⃣ TFTP Server Configuration

📌 Since **xinetd is not used**, TFTP is managed directly by **systemd**.

## 🛠️ Create and Edit TFTP Override Configuration (if needed)

```bash
mkdir -p /etc/systemd/system/tftp.service.d/
```

```bash
vim /etc/systemd/system/tftp.service.d/override.conf
```

## ✍️ Add

```ini
[Service]
ExecStart=
ExecStart=/usr/sbin/in.tftpd -s /var/lib/tftpboot
```

📌 This ensures the TFTP server serves files from `/var/lib/tftpboot`.

---

## ▶️ Enable and Start TFTP Server

```bash
systemctl enable --now tftp.service
```

⚠️ **If you get a `unit not found` error**, enable the socket instead:

```bash
systemctl enable --now tftp.socket
```
---

## 5️⃣ Syslinux for PXE Boot Menu

## 📂 Copy PXE Boot Files

Copy the required **Syslinux PXE bootloader files** into the TFTP root directory:

```bash
cp /usr/share/syslinux/pxelinux.0 /var/lib/tftpboot/
cp /usr/share/syslinux/menu.c32 /var/lib/tftpboot/
cp /usr/share/syslinux/memdisk /var/lib/tftpboot/
cp /usr/share/syslinux/mboot.c32 /var/lib/tftpboot/
cp /usr/share/syslinux/chain.c32 /var/lib/tftpboot/
```

---

## 📁 Create PXE Configuration Directory

```bash
mkdir -p /var/lib/tftpboot/pxelinux.cfg/
```

---

## 📝 Create PXE Boot Menu Configuration

Edit the default PXE menu file:

```bash
vim /var/lib/tftpboot/pxelinux.cfg/default
```

---

## 📜 PXE Menu Content

```cfg
default menu.c32
prompt 0
timeout 600
ONTIMEOUT local
menu title ========= PXE Boot Menu =========

label 1
  menu label ^1) Install CentOS 9
  kernel images/pxeboot/vmlinuz
  append initrd=images/pxeboot/initrd.img method=ftp://192.168.1.37/centos9 devfs=nomount

label 2
  menu label ^2) Boot from local drive
  localboot 0
```

📌 **Explanation:**

* ⏱️ `timeout 600` → 60 seconds wait time
* 🌐 FTP source points to the CentOS 9 installation files
* 🖥️ Option to boot from local disk if PXE is not required

---

## 6️⃣ Firewalld Configuration

⚠️ On **CentOS 9**, always use **firewalld** instead of `iptables`.

## 🔓 Open Required Ports (Firewalld)

Allow all necessary services and ports for the PXE server to function properly:

```bash
firewall-cmd --permanent --add-service=dhcp
firewall-cmd --permanent --add-service=ftp
firewall-cmd --permanent --add-service=tftp
firewall-cmd --permanent --add-port=69/udp
firewall-cmd --permanent --add-port=55000-55999/tcp
firewall-cmd --reload
```

📌 **Explanation:**

* 🧭 **DHCP** → Assigns IP addresses to PXE clients
* 📡 **TFTP (UDP 69)** → Delivers PXE bootloader & kernel
* 📁 **FTP** → Provides OS installation files
* 🔁 **55000–55999/TCP** → Passive FTP ports

---

## 7️⃣ Final Step: Ensure All Services Are Running

Restart all required services to apply changes:

```bash
systemctl restart dhcpd
systemctl restart vsftpd
systemctl restart tftp.service   # or tftp.socket
systemctl restart firewalld
```

---

## ✅ PXE Server Ready 🎉

Your **PXE Boot Server for CentOS 9 Stream** is now ready!

When PXE clients boot, they will receive:

* 🌐 **IP address via DHCP**
* ⚙️ **Bootloader & kernel via TFTP**
* 📦 **OS installation files via FTP**

---

### 🎯 What I Can Do Next for You

* 📘 Combine **everything into a single PXE Boot Server guide**
* ⚡ Add **Kickstart for unattended PXE installs**
* 📄 Export as **README.md / PDF / DOCX**
* 🔧 Troubleshooting checklist (PXE errors & fixes)



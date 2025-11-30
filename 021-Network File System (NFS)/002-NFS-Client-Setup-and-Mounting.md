# 📦 NFS-Client-Setup-and-Mounting

## 📘 NFS Client Setup and Mounting Guide

This section describes how to install NFS client tools, connect to an NFS server, mount shared directories, and optionally set up automatic mounting on boot.

---

# 🛠️ Install NFS Client Utilities

### 🔽 Install required packages depending on your Linux distribution:

---

### 🟦 Debian/Ubuntu:

```bash
apt install rpcbind nfs-common
```

Installs the required RPC service (**rpcbind**) and NFS tools (**nfs-common**).

---

### 🟥 RHEL/CentOS:

```bash
yum install rpcbind nfs-common
```

Same functionality for RHEL/CentOS systems.

---

### (Optional) 👀 Install `showmount` to view exports from an NFS server:

```bash
yum install nfs-utils
```

---

## 🔍 Discover Available NFS Shares

### ✔️ Check available exports from the server:

```bash
showmount -e 192.168.1.31
```

Lists the directories exported by the NFS server at **192.168.1.31**.

---

## 📂 Mount NFS Shares Manually

### ⚠️ Mount the root of the NFS server (not recommended in production):

```bash
mount -t nfs 192.168.1.31:/ /mnt -o nolock
```

---

### 🔻 Unmount the share:

```bash
umount /mnt
```

---

### 📁 Create local directories for mounting multiple shares:

```bash
mkdir /mnt/d1 /mnt/d2
```

---

### 📌 Mount specific shares:

```bash
mount -t nfs 192.168.1.31:/data /mnt/d1/
```

```bash
mount -t nfs 192.168.1.31:/backup /mnt/d2/
```

---

### 📊 Check disk usage and mounts:

```bash
df -h
```

---

### 🔻 Unmount the shares:

```bash
umount /mnt/d1
```

```bash
umount /mnt/d2
```

Here is the **Markdown version with emojis**, matching your screenshot:

---

# 🔗 Auto-Mount NFS Shares via `/etc/fstab`

### ✏️ Edit the file to configure persistent NFS mounts:

```bash
vim /etc/fstab
```

### ➕ Add the following line to mount `/data` on boot:

```bash
192.168.1.31:/data/   /mnt/d1   nfs   rw,sync,hard,intr   0 0
```

---

### ✔️ Apply and verify:

```bash
mount -a
```

```bash
df -h
```
---

## 📦 More Mount Examples

### 🔁 Various mount syntax variations for NFS:

```bash
mount -t nfs 192.168.1.31:/var/armour_share/ /mnt -o nolock
```

```bash
mount -t nfs -o vers=3,nolock 192.168.1.31:/data /mnt/d1/
```

---

## 🧪 Test File Access on Client
Navigate to the mounted directory and test copying files.

### 📂 Navigate to mounted directory and test copying files:

```bash
cd /mnt
ls
```

---

### 👤 Switch user (optional):

```bash
su - armour
```

---

### 📁 Create a subdirectory and copy files:

```bash
cd /mnt/data
mkdir d1
cp -v /etc/passwd d1
```

---

### 🔁 Repeat for another share:

```bash
cd /mnt/backup
mkdir d1
cp -v /etc/passwd d1
```

---

## 📘 Example `/etc/exports` Configuration (on NFS Server)

### These entries define what is shared:

```bash
/data/   192.168.1.0/24(rw,sync,no_root_squash)
/backup/ 192.168.1.0/24(rw,sync,no_all_squash)
```

These export `/data` and `/backup` directories with read-write access and syncing behavior to all clients in the **192.168.1.0/24** subnet.

---
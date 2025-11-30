# 📡 Network-File-System-(NFS)-Server

## 📘 Network File System (NFS) Setup on RHEL/CentOS

This guide walks through installing, configuring, and securing an NFS server on RHEL/CentOS systems, including managing firewall rules with **firewalld** and connecting from an NFS client.

---

## 📦 **Step 1: Install NFS Packages**

### 🔍 Check if NFS-related packages are already installed:

```bash
rpm -qa | grep nfs
```

### 📥 Install **nfs-utils** (core NFS package) and **rpcbind** (used for port mapping):

```bash
yum install nfs-utils rpcbind
```

### 📄 Get package details for **nfs-utils**:

```bash
rpm -qi nfs-utils
```

### 📂 List all files installed by **nfs-utils** package:

```bash
rpm -ql nfs-utils
```

### ⚙️ View configuration files from the package:

```bash
rpm -qc nfs-utils
```

### 📝 Display documentation/changelog files:

```bash
rpm -qd nfs-utils
```

### 🔁 Repeat checks for **rpcbind** package:

```bash
rpm -qi rpcbind
```

---

## 🚀 Enable and Start NFS Services

### 🔧 Enable all essential NFS-related services so they start automatically on system boot:

```bash
systemctl enable rpcbind
```
```bash
systemctl enable nfs-server
```
```bash
systemctl enable nfs-lock
```
```bash
systemctl enable nfs-idmap
```


### ▶️ Start the services immediately:

```bash
systemctl start rpcbind
```
```bash
systemctl start nfs-server
```
```bash
systemctl start nfs-lock
```
```bash
systemctl start nfs-idmap
```
---

## 📂 Step 3: Create a Shared Directory

### 📁 Create the directory to be shared over NFS:

```bash
mkdir -p /data
```

### 🔓 Set permissions to allow full access (use stricter permissions in production):

```bash
chmod 777 /data
```

---


## 📤 Export Shared Directory

### 📝 Edit the NFS exports file to define which directories to share and how:

```bash
vim /etc/exports
```

---

### 📚 Add the following example entry to share **/data** with a local subnet:

```bash
/data/ 192.168.1.0/24(rw,sync)
```

---

### 🔒 Optionally, enable **read-only** access:

```bash
/data/ 192.168.1.0/24(ro,sync)
```

---

### ⚠️ Grant root privileges *(NOT recommended in production)*:

```bash
/data/ 192.168.1.0/24(rw,sync,no_root_squash,no_all_squash)
```

---

### 🔁 Apply the export rules:

```bash
exportfs -rav
```

---

### 🔍 (Optional) Verify active exports and their settings:

```bash
exportfs -v
```

---

### 🔄 Restart NFS service to apply changes:

```bash
systemctl restart nfs-server
```

---

### 🌐 Example: Allow access from any IP (use carefully):

```bash
vim /etc/exports
```

```bash
/data/ *(rw,sync)
```

---


### `/data/` – This is the **local directory on the NFS server** that you're exporting (sharing over the network).

### `*` – This is a **wildcard** that allows **any client (any IP address)** to access the export.

⚠️ **Not recommended for production** — it's wide open and insecure. You should ideally restrict access to trusted IPs or subnets (e.g., `192.168.1.0/24`).

### `rw` – Grants clients **read and write** access to the shared directory.

### `sync` – Ensures that all changes are written to disk **before** a response is sent to the client.

Safer (especially for data integrity) than `async`, but potentially a little slower.

---

## 🔥 Step 5: Open NFS Ports in firewalld

## 📘 Common NFS Service Ports


### **111 – Portmapper / rpcbind**

**Protocol:** TCP/UDP

**Purpose:**

* `rpcbind` (formerly portmapper) maps RPC program numbers to network port numbers.
* Essential for services like `nfs`, `mountd`, and `statd` to communicate over the network.

**Required by:** All RPC-based services including NFS.

---

### **2049 – NFS Server (nfsd)**

**Protocol:** TCP/UDP

**Purpose:**

* The main port used by NFS servers to handle file-sharing requests.
* This is the default and well-known port for NFS.

---

### **20048 – mountd**

**Protocol:** TCP/UDP

**Purpose:**

* Handles initial mount requests from NFS clients.
* Helps negotiate which directories are available to be mounted.
* Port can vary unless fixed using configuration.

---

### 🔥 Enable the firewall service so rules persist after reboot:

```bash
systemctl enable firewalld
```

### ▶️ Start the firewall service:

```bash
systemctl start firewalld
```

### 📡 Allow NFS through the firewall permanently:

```bash
firewall-cmd --permanent --add-service=nfs
```

### 📥 Allow the mount daemon through the firewall:

```bash
firewall-cmd --permanent --add-service=mountd
```

### 🔁 Allow `rpcbind` service:

```bash
firewall-cmd --permanent --add-service=rpc-bind
```

### 🔄 Reload the firewall to apply the new rules:

```bash
firewall-cmd --reload
```

### 👀 (Optional) View the list of active firewall services and zones:

```bash
firewall-cmd --list-all
```

---

## 🔐 Optional: Fixing Ports for Firewalld

### • Manually open individual ports:

```bash
firewall-cmd --permanent --add-port=111/tcp
firewall-cmd --permanent --add-port=111/udp
firewall-cmd --permanent --add-port=2049/tcp
firewall-cmd --permanent --add-port=2049/udp
firewall-cmd --permanent --add-port=20048/tcp
firewall-cmd --permanent --add-port=20048/udp
firewall-cmd --reload
firewall-cmd --list-all
```

---

## 🖥️ NFS Client (Mount Example)

### List exports on the server:
```bash
showmount -e localhost
```

### Check Visibility of exports from the client:
```bash
showmount -e 192.168.1.21
```

### • Mount the shared NFS directory from a client machine:

```bash
mount -t nfs 192.168.1.21:/data /mnt
```
```bash
df -h
Filesystem          Size  Used Avail Use% Mounted on
devtmpfs            4.0M     0  4.0M   0% /dev
tmpfs               1.3G     0  1.3G   0% /dev/shm
tmpfs               529M  7.8M  521M   2% /run
/dev/sda3            15G  6.0G  9.1G  40% /
/dev/sda1           960M  568M  393M  60% /boot
tmpfs               265M   96K  264M   1% /run/user/0
192.168.1.21:/data   18G   15G  3.1G  83% /mnt
```

```bash
cd /mnt/
```
```bash
mkdir testing
```
```bash
ls -lh
```
- Check in the server:
```bash
ls -lh /data
```
#### Output:
```bash
total 0
drwxr-xr-x 2 nobody nobody 6 Nov 25 10:09 testing
```

> Replace **192.168.1.21** with your actual NFS server IP address.
> Ensure `/mnt` exists on the client.

```bash
umount /mnt
```
```bash
df -h
Filesystem      Size  Used Avail Use% Mounted on
devtmpfs        4.0M     0  4.0M   0% /dev
tmpfs           1.3G     0  1.3G   0% /dev/shm
tmpfs           529M  7.8M  521M   2% /run
/dev/sda3        15G  6.0G  9.1G  40% /
/dev/sda1       960M  568M  393M  60% /boot
tmpfs           265M   96K  264M   1% /run/user/0
```
---

### • Optionally : Make the mount (persistent/Permanent) by adding it to `/etc/fstab`:

```bash
192.168.1.21:/data   /mnt   nfs   defaults   0  0
```

```bash
umount /mnt
```

### 📚 Enable `read and Write` access in NFS `Server`:
> /data/ 192.168.1.0/24(rw,sync)


```bash
vim /etc/exports
#/data/ 192.168.1.0/24(rw,sync)
/data/ *(rw,sync)
```
```bash
systemctl restart nfs-server
```
```bash
exportfs -rav
exporting *:/data
```
```bash
showmount -e 192.168.1.21
Export list for 192.168.1.21:
/data 192.168.1.0/24
```
```bash
mount -t nfs 192.168.1.21:/data /mnt
```
```bash
cd /mnt/
```
- Use `Client` for copy data:
```bash
cp -v /etc/passwd /etc/shadow .
'/etc/passwd' -> './passwd'
'/etc/shadow' -> './shadow'
```
- Check `Server` location in data folder
```bash
[/data]
└─# ls -lh
total 8.0K
-rw-r--r-- 1 nobody nobody 2.2K Nov 30 14:18 passwd
---------- 1 nobody nobody 1.2K Nov 30 14:18 shadow
drwxr-xr-x 2 nobody nobody   22 Nov 30 14:06 testing
```
---
## 🔒 Optionally, enable `read-only` access:

> /data/ 192.168.1.0/24(ro,sync)

- Edit Configuration file:
```bash
vim /etc/exports
```
```bash
#/data/ 192.168.1.0/24(rw,sync)
#/data/ *(rw,sync)
/data/ 192.168.1.0/24(ro,sync)
```
```bash
systemctl restart nfs-server
```

```bash
showmount -e 192.168.1.21
Export list for 192.168.1.21:
/data 192.168.1.0/24
```
```bash
mount -t nfs 192.168.1.21:/data /mnt
```
- On `Client` Commands
```bash
cd /mnt/
```
```bash
ls
passwd  shadow  testing
```
### `Copy` message file but show `Read-only file system` limit:
```bash
cp -vr Desktop/ /mnt/testing/
```

- Output: Error
```bash
cp: cannot create directory '/mnt/testing/Desktop': Read-only file system
```

### `Delete` passwd file but show `Read-only` limit:
```bash
rm -rvf passwd 
```

- Output: Error
```bash
rm: cannot remove 'passwd': Read-only file system
```
```bash
ls -lh
```
- Output:
```bash
total 8.0K
-rw-------. 1 root   root      0 Nov 30 15:16 maillog
-rw-------. 1 root   root      0 Nov 30 15:16 maillog-20251109
-rw-------. 1 root   root      0 Nov 30 15:16 maillog-20251118
-rw-------. 1 root   root      0 Nov 30 15:16 maillog-20251125
-rw-------. 1 root   root      0 Nov 30 15:16 maillog-20251130
-rw-r--r--. 1 nobody nobody 2.2K Nov 30 14:18 passwd
----------. 1 nobody nobody 1.2K Nov 30 14:18 shadow
drwxr-xr-x. 2 nobody nobody    6 Nov 30 14:41 testing
```

---
## Grant `root privileges` *(NOT recommended in production)*:

> /data/ 192.168.1.0/24(rw,sync,no_root_squash,no_all_squash)


- Edit Configuration file:
```bash
vim /etc/exports
```
```bash
#/data/ 192.168.1.0/24(rw,sync)
#/data/ *(rw,sync)
#/data/ 192.168.1.0/24(ro,sync)
/data/ 192.168.1.0/24(rw,sync,no_root_squash,no_all_squash)
```
```bash
systemctl restart nfs-server
```
```bash
umount /mnt
```
```bash
mount -t nfs 192.168.1.21:/data /mnt
```
```bash
df -h
```
- Output:
```bash
Filesystem          Size  Used Avail Use% Mounted on
devtmpfs            4.0M     0  4.0M   0% /dev
tmpfs               1.3G     0  1.3G   0% /dev/shm
tmpfs               529M  7.8M  521M   2% /run
/dev/sda3            15G  6.4G  8.6G  43% /
/dev/sda1           960M  568M  393M  60% /boot
tmpfs               265M  100K  264M   1% /run/user/0
192.168.1.21:/data   18G   15G  2.4G  87% /mnt
```
```bash
cp -vr /etc/sudoers /mnt/testing/
'/etc/sudoers' -> '/mnt/testing/sudoers'
```
```bash
ls -lh /mnt/testing/
total 8.0K
-r--r-----. 1 root root 4.3K Nov 30 15:30 sudoers
```
---

## 📝 Notes and Options Explanation

* `rw` – Enables `read-write` access to the share.

* `ro` – Restricts access to `read-only`.

* `sync` – Forces changes to be committed to disk before replying to the client.

* `no_root_squash` – Allows root user from the client to act as root on the server.

* `no_all_squash` – Maintains UID and GID from client instead of mapping them to ‘nobody’.

---
## 🟢 Optional: `SELinux` Context

### • Temporarily apply an SELinux label allowing NFS sharing:

```bash
chcon -Rt nfs_t /data
```

### • To make the context permanent:

```bash
semanage fcontext -a -t nfs_t "/data(/.*)?"
restorecon -Rv /data
```

---

## 🟢 Optional: Troubleshooting Tips

### ✔️ Check the status of the NFS server:

```bash
systemctl status nfs-server
```

### ✔️ View system log entries for errors:

```bash
journalctl -xe
```

### ✔️ List exports on the server:

```bash
showmount -e localhost
```

### ✔️ Check visibility of exports from the client:

```bash
showmount -e 192.168.1.33
```

---
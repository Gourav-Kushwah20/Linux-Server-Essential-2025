# 📁 Passwd-File-Linux-User-Account-File

## `/etc/passwd` – Linux User Account File

The `/etc/passwd` file stores **basic user account information** on Linux systems. Each line defines a single user in a **colon-separated format**.

---

## 📝 Format of `/etc/passwd`

`username : x : UID : GID : comment : home_directory : shell`


| Field No | Field Name       | Description                                               |
|----------|------------------|-----------------------------------------------------------|
| 1        | `username`       | Login name (e.g., `root`, `armour`)                      |
| 2        | `x`              | Indicates the actual password is stored in `/etc/shadow` |
| 3        | `UID`            | User ID: `0` is root, `1000+` are normal users           |
| 4        | `GID`            | Primary group ID                                          |
| 5        | `comment`        | GECOS field (user’s real name or info)                   |
| 6        | `home_directory` | Default login directory                                   |
| 7        | `shell`          | Default shell (e.g., `/bin/bash`, `/sbin/nologin`)       |

---

## 🧩 Example Entry Breakdown

`root : x : 0 : 0 : root : /root : /bin/bash`


| Field     | Value     | Description                          |
|-----------|-----------|--------------------------------------|
| Username  | `root`    | Superuser account                    |
| Password  | `x`       | Password stored in `/etc/shadow`    |
| UID       | `0`       | User ID for root                    |
| GID       | `0`       | Group ID for root                   |
| Comment   | `root`    | GECOS (optional description)        |
| Home Dir  | `/root`   | Home directory                      |
| Shell     | `/bin/bash` | Default shell for user            |

---

## 📂 Sample `/etc/passwd` Entries

```
daemon:x:2:2:daemon:/sbin:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/usr/share/empty.sshd:/usr/sbin/nologin
apache:x:48:48:Apache:/usr/share/httpd:/sbin/nologin
armour:x:1000:1000:Armour Infosec:/home/armour:/bin/bash
```

| User    | UID   | GID   | Comment                 | Home Directory           | Shell              |
|---------|-------|-------|-------------------------|---------------------------|--------------------|
| daemon  | 2     | 2     | daemon                  | `/sbin`                   | `/sbin/nologin`    |
| sshd    | 74    | 74    | Privilege-separated SSH | `/usr/share/empty.sshd`   | `/usr/sbin/nologin`|
| apache  | 48    | 48    | Apache                  | `/usr/share/httpd`        | `/sbin/nologin`    |
| armour  | 1000  | 1000  | Armour Infosec          | `/home/armour`            | `/bin/bash`        |

> ⚠️ **Note:** System or service users often use `/sbin/nologin` to prevent shell access for security purposes.

---

## 🔍 Viewing `/etc/passwd`

### Show all entries:
```sh
cat /etc/passwd
```

### Show only human users (UID ≥ 1000):
```sh
awk -F: '$3>=1000 && $3<65534 { print $1 }' /etc/passwd
```

### Search for a specific user:
```sh
grep "username" /etc/passwd
```

---

## ✏️ Editing `/etc/passwd`

> Open the file safely with root privileges.
```sh
vim /etc/passwd

root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
nobody:x:65534:65534:Kernel Overflow User:/:/sbin/nologin
systemd-coredump:x:999:999:systemd Core Dumper:/:/sbin/nologin
dbus:x:81:81:System message bus:/:/sbin/nologin
polkitd:x:998:998:User for polkitd:/:/sbin/nologin
avahi:x:70:70:Avahi mDNS/DNS-SD Stack:/var/run/avahi-daemon:/sbin/nologin
rtkit:x:172:172:RealtimeKit:/:/sbin/nologin
libstoragemgmt:x:996:996:daemon account for libstoragemgmt:/:/usr/sbin/nologin
geoclue:x:995:995:User for geoclue:/var/lib/geoclue:/sbin/nologin
tss:x:59:59:Account used for TPM access:/:/usr/sbin/nologin
colord:x:994:994:User for colord:/var/lib/colord:/sbin/nologin
sssd:x:993:993:User for sssd:/:/sbin/nologin
clevis:x:992:992:Clevis Decryption Framework unprivileged user:/var/cache/clevis:/usr/sbin/nologin
setroubleshoot:x:991:991:SELinux troubleshoot server:/var/lib/setroubleshoot:/usr/sbin/nologin
pipewire:x:990:990:PipeWire System Daemon:/run/pipewire:/usr/sbin/nologin
flatpak:x:989:989:Flatpak system helper:/:/usr/sbin/nologin
gdm:x:42:42:GNOME Display Manager:/var/lib/gdm:/usr/sbin/nologin
gnome-initial-setup:x:988:987::/run/gnome-initial-setup/:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/usr/share/empty.sshd:/usr/sbin/nologin
chrony:x:987:986:chrony system user:/var/lib/chrony:/sbin/nologin
dnsmasq:x:986:985:Dnsmasq DHCP and DNS server:/var/lib/dnsmasq:/usr/sbin/nologin
tcpdump:x:72:72::/:/sbin/nologin
sec-learner:x:1000:1000:Sec-learner:/home/sec-learner:/bin/bash
```
## 🛡️Backup Before Editing
```sh
cp /etc/passwd /etc/passwd.txt
```
✅ Prefer using `usermod`,and `userdel` over manual editing.
---
## ⚠️ Important Noets
- 🚫 Never delete or corrupt `/etc/passwd `- It can make your system unbootable.
- 🛠️ use `vipw` (a safe editor) to prevent file corruption.
- ❌ Do not give naormal users UID (reserved for `root`)
# Share-With-Selected-Users 🔐

## Share With Selected Users 👥

This setup enables **controlled access** to the Samba share (`/backup`) by specifying selected users. 🔧

---

## 🛠️ Configure Samba Shared Folders

Edit the Samba configuration file:

```bash
vim /etc/samba/smb.conf
```


Update or append the following sections in the configuration:

---

```ini
[global]
    workgroup = SAMBA
    security = user
    passdb backend = tdbsam

    printing = cups
    printcap name = cups
    load printers = yes
    cups options = raw

[homes]
    comment = Home Directories
    valid users = %S, %D%w%S
    browseable = No
    read only = No
    inherit acls = Yes

[printers]
    comment = All Printers
    path = /var/tmp
    printable = Yes
    create mask = 0600
    browseable = No

[print$]
    comment = Printer Drivers
    path = /var/lib/samba/drivers
    write list = @printadmin root
    force group = @printadmin
    create mask = 0664
    directory mask = 0775

[data]
    comment = Data
    path = /opt/data
    public = yes
    writable = yes
    guest ok = no
    guest only = no

[backup]
    comment = Server Backup
    path = /backup
    public = yes
    writable = yes
    valid users = infosec, sec-learner
```
The `valid users` directive ensure only infosec and `sec-learner` can access the `backup` share.

---
## 🔄 Restart Samba Service

- Apply the configuration changes:

```bash
systemctl restart smb.service
```

---

## 🔒 Access The Restricted Share 

### 🚫 Access as **thw** (should be denied if not listed in `valid users`) 🚫

```bash
smbclient //192.168.1.37/backup -U smb-user1
```

---

### Access as allowed users ✔️

```bash
smbclient -L 192.168.1.37 -U infosec
```

```bash
smbclient -L 192.168.1.37 -U armour
```

---

## 📝 Additional Notes 

* Make sure the users (**infosec**, **armour**) exist as both **Linux system users** and **Samba users**. 👥
* Add Samba users with:

```bash
smbpasswd -a infosec
```

```bash
smbpasswd -a armour
```

---

* Ensure the shared directory `/backup` has proper ownership/permissions: 🔧

```bash
mkdir -p /backup
```

```bash
chmod -R 770 /backup
```

```bash
chown root:users /backup
```

---

> 💡 *You can also create a custom group (e.g., `sambashare`) and add **infosec** and **armour** to it for easier permission management.*

---

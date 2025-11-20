# Anonymous-Samba-Share 🌐

## Anonymous Samba Share 👤➡️📁

Anonymous or guest shares allow users to access shared directories **without authentication**.
This setup is useful for **public access within a trusted network**. 🏠🔓

---

## Prepare Public Directory 📂✨

Create a public directory with full access permissions:

```bash
mkdir /Public
```

Set open permissions for read, write, and execute access:

```bash
chmod 777 /Public/
```

Or recursively apply to all contents inside:

```bash
chmod -R 777 /Public/
```

Verify directory exists and has correct permissions:

```bash
ls -lh / | grep Public
```

Set ownership to **nobody** user and group:

```bash
chown -R nobody:nobody /Public/
```

---

## ⚙️ Configure Samba For Anonymous Access

Edit the Samba configuration file:

```bash
vim /etc/samba/smb.conf
```

```ini
[Public]
    path = /Public
    writable = yes       # ✏️ Allow writing
    guest ok = yes       # 👤 Guest access allowed
    guest only = yes     # 🚪 Only guest users allowed
    read only = no       # 📖❌ Not read-only
    create mode = 0777   # 📁 New file permissions
    directory mode = 0777 # 📂 Directory permissions
    force user = nobody  # 👤 Force operations as 'nobody'
```

### This configuration:

* ✔️ Enables **guest-only access**
* 📑 Allows **full read/write/delete** capabilities
* 👤 Forces all operations as **nobody** user

---

## 🔄 Restart Samba Service

Apply the configuration by restarting Samba:

```bash
systemctl restart smb.service
```

---

## 🌐 Test Anonymous Share Access

### Check the list of available shares on the Samba server:

```bash
smbclient -L 192.168.1.21
```

### Try accessing the public share anonymously:

```bash
smbclient //192.168.1.21/Public/
```

---

## 🔓 Guest Access Examples

### Explicitly use guest access with no credentials:

```bash
smbclient -L 192.168.1.33 -U "" -N
```

### Or connect directly to the public share as a guest:

```bash
smbclient //192.168.1.33/Public -U "" -N
```

### Another equivalent form:

```bash
smbclient -U "" //192.168.1.27/Public -N
```

---

## 🧩 Additional Sample Configuration

Here's an extended **smb.conf** snippet with additional shares:

```bash
mkdir /opt/data
```

```bash
mkdir /backup
```

```bash
vim /etc/samba/smb.conf
```

### Sample smb.conf entries:

```ini
[data]
    comment = Data
    path = /opt/data
    writable = yes

[backup]
    comment = Server Backup
    path = /backup
    writable = yes

[Public]
    path = /Public
    writable = yes 
    guest ok = yes 
    guest only = yes 
    read only = no
    create mode = 0777 
    directory mode = 0777 
    force user = nobody 
``` 

Make sure the directories **/opt/data** and **/backup** exist with proper ownership and permissions for listed users. ✔️

```bash
systemctl restart smb.service
```

---

## 🔐 Security Note

Anonymous shares should only be used in **trusted networks**.
Avoid enabling guest access on **internet-facing systems** or in environments where data sensitivity is high. ⚠️

Let me know if you’d like to **automate this setup with a shell script** or **expand it into a Samba AD configuration**! 💡

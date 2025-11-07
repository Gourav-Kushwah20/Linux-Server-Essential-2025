# 📁 /etc/group — Linux Group Account File

The `/etc/group` file defines **group memberships** on Linux systems. Groups are used to manage permissions and access controls for users collectively.

---

## 🧾 Format of `/etc/group`
Each line in `/etc/group` follows this **colon-separated** format:

```
group_name:x:GID:user_list
```

| Field No. | Field Name  | Description                                                                 |
|-----------|-------------|-----------------------------------------------------------------------------|
| 1         | `group_name`| Name of the group (e.g., `wheel`, `sshd`)                                  |
| 2         | `x`         | Placeholder for encrypted group passwords (rarely used)                    |
| 3         | `GID`       | Group ID – a unique numeric identifier                                      |
| 4         | `user_list` | Comma-separated list of group members (can be empty)                        |

---

## 📘 Example Entries
```
cat /etc/group

root:x:0:
daemon:x:2:
sshd:x:74:
apache:x:48:
armour:x:1000:armour
```

---

## 🔍 Breakdown of Entry: `armour:x:1000:armour`

| Field       | Value    | Description             |
|-------------|----------|-------------------------|
| Group Name  | `armour` | Name of the group       |
| Password    | `x`      | Placeholder (unused)    |
| GID         | `1000`   | Group ID                |
| User List   | `armour` | Members of the group    |

---
## 👁️ View Group File

View Full group list:

```
cat /etc/group
```
View Specific group:

```bash
getent group groupname

getent group root # like this
```

View groups for a specific user:

```bash
group username
```
---
## 📝Editing /etc/group
use a text editor with root priviledge:

```bash
vim /etc/group
```

## Backup first:

```
cp /etc/group /etc/group.bak
```
---
## 🛠️ Common Group Management Commands

- ➕ Create a new group

```bash
groupadd devteam
```
- 👤 Add a user to a group

```bash
usermod -aG devteam username
```
- ❌ Remove a user from a group

```bash
gpasswd -d username devteam
```
- 🔁 Change group name

```bash
groupmod -n newname oldname
```

- 🗑️ Delete a group

```bash
groupdel devteam
```

---

## 📆 System Groups

| Group Name       | Purpose                    |
| ---------------- | -------------------------- |
| `wheel` / `sudo` | Grant sudo/root access     |
| `audio`          | Audio device access        |
| `cdrom`          | CD/DVD device access       |
| `ftp`            | FTP-only access            |
| `sshd`           | Used by SSH daemon         |
| `postfix`        | Mail server group          |
| `kvm`, `libvirt` | Virtualization permissions |
| `pulse`          | PulseAudio runtime group   |

> *System-defined groups help control access to hardware and services without giving full administrative rights.*

---

## ✅ Summary

| Task                        | Command Example              |
| --------------------------- | ---------------------------- |
| View groups                 | `cat /etc/group`             |
| Check user group membership | `groups username`            |
| Add user to group           | `usermod -aG group username` |
| Remove user from group      | `gpasswd -d user group`      |
| Create a new group          | `groupadd newgroup`          |

---

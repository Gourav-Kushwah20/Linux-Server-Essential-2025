
# 🔒 `/etc/shadow` – Secure User Passwords File

The `/etc/shadow` file stores **encrypted user password data** and **password aging policies**.  
📛 It is **readable only by the root user** and is critical for system authentication.

---

## 🧾 Format of `/etc/shadow`

Each line represents one user, with fields separated by colons `:`:

```
username : encrypted_password : last_change : min : max : warn : inactive : expire : unused
```
```
root : $6$yfiH/uJof5kZXkSK$PC3B6kWVh/hu5TOtxYNNFApFNnY0RNKDgnuM/IFFxqdlR81gYjQZEm4oaWr9SXPZnTWMJDG9R9jQsDc5kfsCE1 : : 0 : 99999 : 7 : : :
```

| #️⃣ Field No | 🏷️ Field Name         | 📝 Description                                                |
|-------------|------------------------|----------------------------------------------------------------|
| 1           | `username`             | 👤 Login name                                                  |
| 2           | `encrypted_password`   | 🔐 Password in hash format or placeholder (`*`, `!`, `!!`)     |
| 3           | `last_change`          | 📅 Days since Jan 1, 1970 when password was last changed       |
| 4           | `min`                  | ⏳ Minimum days before password can be changed                 |
| 5           | `max`                  | 📆 Maximum days a password is valid                            |
| 6           | `warn`                 | ⚠️ Days before expiry to warn user                            |
| 7           | `inactive`             | 📴 Days after expiry until account is disabled                 |
| 8           | `expire`               | 🚫 Account expiration date (days since epoch)                  |
| 9           | `unused`               | 🗃️ Reserved for future use                                    |

---

## 🧪 Password Hash Types

The second field (`encrypted_password`) uses the following format:

```
$type$salt$hashed_password
```
## 🔐 Password Hash Types

The second field (encrypted password) uses the following format:

```
$type$salt$hashed_password
```

| Type Prefix | Algorithm                      |
|-------------|--------------------------------|
| `$1$`       | MD5 (insecure)                 |
| `$2a$`      | Blowfish                       |
| `$2y$`      | Eksblowfish                    |
| `$5$`       | SHA-256                        |
| `$6$`       | SHA-512 (default on most systems) |

> A value of `*` or `!` disables the password login for the account.

---

## 📘 Example Entries

```
root:$6$TDKFHGucRcYEMKuSt4DBGsvhWS80a1z/...:0:99999:7:::
daemon:*:17110:0:99999:7:::
sshd:*:12004:0:99999:7:::
apache:*:20063:0:99999:7:::
armour:$6$vdsYGa7whThy1YU/...:0:99999:7:::
```

---

## 🔎 Entry Breakdown (Example: `daemon`)

```
daemon:*:17110:0:99999:7:::
```

| Field       | Value     | Description                                         |
|-------------|-----------|-----------------------------------------------------|
| Username    | `daemon`  | System user                                         |
| Password    | `*`       | No login allowed                                    |
| Last Change | `17110`   | Days since epoch (converted via `date -d '1970-01-01 +1710 days'`) |
| Min Age     |  `0`      | Can change password any time                        |
| Max Age     |  `99999`  | Password never expires                              |
| Warn        |  `7`      | Warn 7 days before expiration                       |
| Inactive    |  empty    | Not Set                                             |
| Expire      |  empty    | Not Set                                             |

---

## 📄 Useful Commands

- View current password aging settings
```bash
chage -l root

Last password change				                : never
Password expires					                : never
Password inactive					                : never
Account expires						                : never
Minimum number of days between password change		: 0
Maximum number of days between password change		: 99999
Number of days of warning before password expires	: 7
```
- Set password expiration policy

```bash
chage -M 90 -W 7 -m | username
```
- `-M 90`: Max 90 Days
- `-W 7` : Warn 7 days before
-  `-m 1`: Min 1 day before change

---

## 📊 Check /etc/login.defs Defaults

- View password Policy parameters

```bash
grep PASS /etc/login.defs
```

- Check encrypted algorithm(e.g SHA-512)
```bash
grep SHA /etc/login.defs
```

---

## ✍️ Editing /etc/shadow

Only root can edit:

```bash
vim /etc/shadow
```
```bash
cp /etc/shadow /etc/shadow.txt
```

---
## 🔒Security Considerations
- Do not share or expose `/etc/shadow` - It contains sensitive password hashes.
- Avoid using weak algorithm like MD5.
- Use `passwd`, `chage`,and `usermod` to manage safety instead of manual edits.

---
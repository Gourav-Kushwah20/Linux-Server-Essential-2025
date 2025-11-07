
## 🔐 Creating Users with Encrypted Passwords

You must provide an encrypted password hash when using the `-p` option with `useradd`. 

Below are safe methods to generate and assign hashed passwords.

### ✅ Using OpenSSL (MD5 and SHA-512)

🔸 Generate MD5 hash:
```bash
openssl passwd 123
```
Example Output:
```bash
$1$TSLtPv6V$0jm8nlyqmD.hy0P0glSZh/
```
🔸 Create user u22 with the hash:
```bash
useradd -p '$1$TSLtPv6V$0jm8nlyqmD.hy0P0glSZh/' u22
```
### 🔐 Now you can log in graphically with:

- Username: u22

- Password: 123

🔸 Generate SHA-512 hash:
```bash
openssl passwd -6 123
```
Example Output:
```bash
$6$Ad2V4Im7X8sdRSx4$lV90yKkW98g859dcnm1FqPXF.dCBtk6Do/FkoUzLxbBHj.FG/wxQfGmy2S3eoDnj5SEk6tKAuYHofp4RO8/wu0
```
🔸 Create user `u23` with `SHA-512 `password:
```bash
useradd -p '$6$Ad2V4Im7X8sdRSx4$lV90yKkW98g859dcnm1FqPXF.dCBtk6Do/FkoUzLxbBHj.FG/wxQfGmy2S3eoDnj5SEk6tKAuYHofp4RO8/wu0' u23
```
🖥️ You can now log in graphically as `u23` using password `123`.

## ✅ Using Python to Generate SHA-512 Password

🔸 Generate SHA-512 encrypted password with Python:
```bash
python -c 'import crypt; print(crypt.crypt("123",crypt.mksalt(crypt.METHOD_SHA512)))'
```
Example Output:
```bash
$6$ePiN6ARw449bBOnx$xgQv5on8/kf08KMO9PfO3R78M8JvlAdHj3Uje8gvVYKWqD9jgzOfyX4GqHVbOCc1YEUJLpLm/Hlg/6S94fkCT1
```

🔸 Create user u24 with Python-generated password:
```bash
useradd -p '$6$ePiN6ARw449bBOnx$xgQv5on8/kf08KMO9PfO3R78M8JvlAdHj3Uje8gvVYKWqD9jgzOfyX4GqHVbOCc1YEUJLpLm/Hlg/6S94fkCT1' u24
```
🖥️ You can now log in graphically as u24 with password 123.

---
## 🔐 Creating Encrypted Passwords Using mkpasswd (and Full User Creation)
### 🔧 1. Install Required Tools

```bash
yum install expect
yum install mkpasswd
```
These tools are used to:

- `mkpasswd` – generate encrypted password hashes.

- `expect` – automate interactive password prompts (optional, advanced scripting).
## 🔐 2. Generate Encrypted Passwords Using mkpasswd
#### SHA-512 (Recommended):

```bash
mkpasswd -m sha-512
Password:123
```
You’ll be prompted to enter a password (e.g., `123`).

Example Output:
```bash
$6$qA7LRnjteg959hmX$TyUu/3ZBxvp7RghU634r/0L9S.0Q6mseEpOnlIsjwKrxG87cCoEOjCRBe/fjZbz59Cq23jQrbNBNxtRUIkifo1
```

#### MD5 (Legacy — not recommended):
```bash
mkpasswd -m md5
Password:
```
Example Output:
```bash 
$1$zqlHrGRq$Uh6jPzKDfPP8RSKRJ3nLw/
```
## 👤 3. Create a Full-Featured User with Custom Options
```bash
useradd -d /backup/u26 -s /usr/bin/bash -u 1030 -c "Full user details" -p '$6$qA7LRnjteg959hmX$TyUu/3ZBxvp7RghU634r/0L9S.0Q6mseEpOnlIsjwKrxG87cCoEOjCRBe/fjZbz59Cq23jQrbNBNxtRUIkifo1' u26
```
### 🔍 What Each Option Does:
- -d /backup/u26 → Sets the user's home directory.

- -s /usr/bin/bash → Sets the login shell.

- -u 1030 → Sets a custom UID.

- -c "Full user details" → Adds a comment (visible in /etc/passwd).

- -p '...' → Sets the encrypted password.

- u26 → The username.

--- 
## 👤 Additional useradd Examples and Options
### 1️⃣ Create a User with Group Membership and Full Config
```bash
useradd -m -d /home/u29 -s /bin/bash -G wheel u29
```
- -m → Create home directory.

- -d → Custom home directory (/home/u29).

- -s → Set login shell.

- -G wheel → Add to wheel (sudo) group.

Look for wheel group to see u29 as a member.

### 🔍 Verify in /etc/gshadow:
```bash
cat /etc/gshadow
```

2️⃣ Create a User with Expiry Date
```bash
useradd u30 -e 2025-12-31
```
- `-e` → Set account expiration date.

User `u30` will be disabled automatically after Dec 31, 2025.

🔍 Check in `/etc/shadow` for expiry entry.

### 3️⃣ Create a System User
```bash
useradd -r sys_u1
```
`-r` → Creates a system account (usually UID < 1000).

Used for services and background processes.

### 4️⃣ Set Inactivity Days After Password Expiry
```bash
useradd -f 20 u31
```
`-f 20` → Account is disabled 20 days after password expires (in` /etc/shadow`).

5️⃣ Primary vs. Supplementary Group Assignment
Set Primary Group by GID:
```bash
useradd -g 1024 it1
```
- `-g` → Sets primary group for user `it1`.
### Add to Supplementary Group:

```bash
useradd -G 1024 it2
```
- `-G` → Adds user `it2` to supplementary group `1024` (does not affect primary group).

### 6️⃣ Do Not Create a Group with the Same Name
```bash
useradd -N u27
```
`-N` → Do not create a user-private group (default behavior is to create one).

Instead, the user is added to a default group (usually `users` or GID from `/etc/default/useradd`).


# 🔐 SSH-Public-And-Private-Key-Configuration

## 🔑 SSH Public and Private Key Configuration

This document explains how to generate SSH public and private keys, configure **sshd**, and set up secure SSH access for both the root user and a custom user (**sec-learner**). Each section includes commands and descriptions for clarity.

---

## 🗝️ SSH Key Generation And Server Configuration

## 📝 Edit SSH Daemon Configuration

Open the SSH daemon configuration file to enable public key authentication and make necessary changes:

```bash
vim /etc/ssh/sshd_config
```

Ensure the following options are set:

```bash
PermitRootLogin yes
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
PasswordAuthentication no
ChallengeResponseAuthentication no
UsePAM yes
```

> 🔒 These settings enable key-based authentication and disable password-based login for added security.

---

## 🛠️ Generate SSH Key Pair (RSA)

Use the **ssh-keygen** tool to generate a key pair:

```bash
ssh-keygen --help
```

Generate an RSA key pair:

```bash
ssh-keygen -t rsa
```

This creates:

* **id_rsa** (private key)
* **id_rsa.pub** (public key)

stored in `~/.ssh/`.

```bash
file id_rsa              
```
- Output: id_rsa: OpenSSH private key

```bash
file id_rsa.pub 
```
- Output: id_rsa.pub: OpenSSH RSA public key

---

## 🔑 Authorize SSH Key For Root Login

### 📁 Navigate to the `.ssh` directory:

```bash
cd /root/.ssh/
```

View the key files:

```bash
file id_rsa
file id_rsa.pub
```

---

### 📌 Copy the public key to the `authorized_keys` file:

```bash
cp -v /root/.ssh/id_rsa.pub /root/.ssh/authorized_keys
```

> 🔐 This file is used by **sshd** to validate login attempts via public key.

---

### 🔄 Restart SSH Service

Apply the configuration changes:

```bash
systemctl restart sshd.service
```

---

### 🔍 Verify SSH Service Status

Check if SSH is listening on the correct port:

```bash
netstat -nltup | grep sshd
```

---

### 🧰 ssh-add

`ssh-add` is used to add, list, and manage SSH private keys stored in the **ssh-agent**, which helps avoid repeated passphrase prompts during SSH authentication.

Here is the **Markdown version with emojis**, accurately matching your screenshot:

---

## 🧰 Basic Usage of `ssh-add`

| **Command**                     | **Description**                                                             |
| ------------------------------- | --------------------------------------------------------------------------- |
| `ssh-add`                       | Adds default private keys (`~/.ssh/id_rsa`, `id_ecdsa`, etc.) to the agent. |
| `ssh-add /path/to/key`          | Adds a specific private key.                                                |
| `ssh-add -l`                    | Lists fingerprints of all currently loaded keys.                            |
| `ssh-add -L`                    | Lists public keys currently held by the agent.                              |
| `ssh-add -d /path/to/key`       | Removes a **specific** key from the agent.                                  |
| `ssh-add -D`                    | Deletes **all** keys from the agent.                                        |
| `ssh-add -c /path/to/key`       | Adds key and requires user confirmation for each use.                       |
| `ssh-add -t 3600 /path/to/key`  | Adds key with a 1-hour lifetime (3600 seconds).                             |
| `ssh-add -s /path/to/pkcs11.so` | Adds a key from a PKCS#11 smartcard/token provider.                         |
| `ssh-add -e /path/to/pkcs11.so` | Removes a key from a PKCS#11 provider.                                      |

---
## Basic usage
### 🚀 Start ssh-agent and Add a Private Key

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

---

### 📜 List All Public Keys Loaded

```bash
ssh-add -L
```

---

### ❌ Remove a Specific Key

```bash
ssh-add -d ~/.ssh/id_rsa
```

---

### 🔥 Remove All Keys From Agent

```bash
ssh-add -D
```

---

### ⏱️ Add a Key With 10-Minute Timeout

```bash
ssh-add -t 600 ~/.ssh/id_ed25519
```

---

### 🔐 Add a Key and Ask Confirmation Before Every Use

```bash
ssh-add -c ~/.ssh/id_rsa
```

Here is the content rewritten in clean **Markdown format with emojis**:

---

## 🌍 Using a Private Key to Connect From Another Machine

### 📝 Create and configure a private key file locally:

```bash
vim id_rsa_centos
```

---

### 🔏 Set the correct permission:

```bash
chmod 600 id_rsa_centos
```

---

### 🔗 Connect to the server using the private key:

```bash
ssh -i id_rsa_centos root@192.168.1.38
```


---

## 🔐 DSA Key Pair For Custom User

This section demonstrates how to generate and configure a **DSA key pair** for a non-root user.

---

## 🛠️ Generate DSA Key Pair

```bash
ssh-keygen -t dsa -f /tmp/id_dsa -q -N "armour123"
```

➡️ This generates:

* `/tmp/id_dsa` (private key)
* `/tmp/id_dsa.pub` (public key)

---

## 📁 Prepare SSH Directory for User

### Create the `.ssh` directory:

```bash
mkdir /home/armour/.ssh
```

### Verify the directory:

```bash
ls -lha /home/armour/ | grep .ssh
```

---

## 🔑 Copy Public Key to Authorized Keys

```bash
cp /tmp/id_dsa.pub /home/armour/.ssh/authorized_keys
```

### Check the file:

```bash
ls -lha /home/armour/.ssh/
```

---

### 🛡️ Set Correct Ownership

```bash
chown -R armour:armour /home/armour/.ssh/
```

---

### 🔗 Use DSA Key to Connect

Edit and configure the private key:

```bash
vim id_dsa_armour
```
---

## 🔒 Set Proper Permissions

```bash
chmod 600 id_dsa_armour
```

## 🌐 Use the Key to Connect

```bash
ssh -i id_dsa_armour armour@<hostname or IP>
```

---

## 🧩 SSH Daemon Configuration Example

Below is a simplified but secure configuration example for **`/etc/ssh/sshd_config`**:

```bash
PermitRootLogin yes
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
PasswordAuthentication no
ChallengeResponseAuthentication no
UsePAM yes
X11Forwarding yes
Subsystem sftp /usr/libexec/openssh/sftp-server
```

> 💡 *This configuration is ideal for disabling passwords and enforcing public key authentication.*

---

## 📝 Additional Tips

### 🔐 Security Best Practices

* 🔑 Always use strong passphrases when generating private keys.
* 🙅‍♂️ Never share private keys — they must remain secret.

### 🛡️ Hardening SSH

* 🔄 Change the default SSH port (22) if you want to reduce common scans.
* 🧱 Consider using **fail2ban** or a firewall (like `ufw` or `iptables`) to reduce brute-force attempts.

### 💾 Backup & Safety

* 📦 Backup `.ssh` directories securely for disaster recovery.

---

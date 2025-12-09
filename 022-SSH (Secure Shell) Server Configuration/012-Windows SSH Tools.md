
# 🪟 Windows SSH Tools

## 🔹 PuTTY and PuTTYgen

* **PuTTY:** A Windows SSH client supporting SSH, Telnet, serial, and SCP connections.
* **PuTTYgen:** Tool to generate or convert SSH keys to the **.ppk** format used by PuTTY.

---

## 🔹 WinSCP

* GUI-based SSH file transfer client for Windows.
* Supports SFTP and SCP protocols.
* Allows drag-and-drop transfers and uses **.ppk** key files.

---

# 🚀 Start SSH Agent and Add Key

Keeps private keys in memory to avoid retyping passphrases.

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

---

# 🏷️ Use SSH Config For Aliases

Create an SSH config file for convenient short commands.

```text
Host devserver
    HostName 192.168.1.33
    User root
    Port 2222
    IdentityFile ~/.ssh/id_rsa
    LocalForward 8080 localhost:80   # Example port forwarding
    ProxyJump gateway.example.com    # Example jump host (proxy)
```

Now you can connect easily with:

```bash
ssh devserver
```
---

## 🚫 Disable Host Key Checking (For Automation Only)

Suppress host key prompts in automated or non-interactive environments (not secure).

```bash
ssh -o StrictHostKeyChecking=no root@192.168.1.100
```

---

## 🛡️ SSH Security Tips

* 🔑 Use public/private key authentication over passwords.
* 🚫 Set **PermitRootLogin no** to prevent direct root logins.
* 🎯 Limit access using **AllowUsers** and firewall rules.
* 🛑 Enable **fail2ban** to ban IPs with repeated failed logins.
* 🔄 Regularly update the OpenSSH server and client software.
* ⚠️ **Disable weak ciphers and use strong key exchange algorithms:**

```text
Ciphers aes128-ctr,aes192-ctr,aes256-ctr
KexAlgorithms diffie-hellman-group14-sha1
```

---

# 🔑 SSH-Keygen Usage and SSH Authentication Setup

This document provides a comprehensive guide to generating SSH keys, configuring public key authentication, setting appropriate permissions, and connecting to remote servers using private keys.
It includes examples for both RSA and DSA key generation and usage.

---

## 🧰 Generate a Basic SSH Key Pair

```bash
ssh-keygen
```

➡️ This command generates a default RSA key pair and stores it in:

* `~/.ssh/id_rsa`
* `~/.ssh/id_rsa.pub`

---

## 🔐 Generate an RSA Key Without a Passphrase

```bash
ssh-keygen -t rsa -q -N ""
```

➡️ This creates an RSA key with **no passphrase** for passwordless login.

---

## 📥 Add the Public Key to Authorized Keys

Copies the public key to the list of authorized keys on the local system to allow SSH login.

```bash
cp /root/.ssh/id_rsa.pub /root/.ssh/authorized_keys
```

---

## 🌐 SSH Into a Server Using a Custom Key

Specify the key file while connecting to a remote server:

```bash
ssh -i id_rsa_centos root@192.168.1.38
```

---

## 🔧 Generate RSA Keys with Different Bit Sizes and Passphrases

### 🔒 2048-Bit RSA Key With a Passphrase

```bash
ssh-keygen -b 2048 -t rsa -f /tmp/id_rsa -q -N "armour123"
```

---


## 🔐 2048-Bit RSA Key Without a Passphrase

```bash
ssh-keygen -b 2048 -t rsa -f /tmp/id_rsa1 -q -N ""
```

---

## 📂 Default Key Location With a Passphrase

```bash
ssh-keygen -b 2048 -t rsa -f /root/.ssh/id_rsa -q -N "armour123"
```

---

## 📝 Simple RSA Key Generation With Filename and Passphrase

```bash
ssh-keygen -t rsa -f /tmp/id_rsa3 -q -N "armour123"
```

---

## 🔍 Check the Key File Type

Check the type of the generated private and public key files:

```bash
file id_rsa
```

```bash
file id_rsa.pub
```

---

## 🌐 SSH Using Specific Key and Key Type Option

Use a custom identity file and specify acceptable public key types:

```bash
ssh -o PubkeyAcceptedKeyTypes=ssh-rsa root@192.168.1.33 -i id_rsa
```

---

## 🛠️ Generate a DSA Key

## 🔓 1024-Bit DSA Key Without a Passphrase

```bash
ssh-keygen -b 1024 -t dsa -f /tmp/id_dsa -q -N ""
```

## 🔐 DSA Key With a Passphrase

```bash
ssh-keygen -t dsa -f /tmp/id_dsa -q -N "armour123"
```

---

## ➕ Add DSA Public Key to Authorized Keys

```bash
cp id_dsa.pub /root/.ssh/authorized_keys
```

---

## 🔏 Set Proper Permissions on Authorized Keys

```bash
chmod 644 /root/.ssh/authorized_keys
```

---

## 🔑 Example Private Key

This is an example of an RSA private key block.
📁 Save this in a file like `id_rsa` and secure it with:

```bash
chmod 600 id_rsa
```


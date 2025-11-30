# 🔐 Change-Default-SSH-Port

## 🔄 Change Default SSH Port

Changing the default SSH port (from the default **22**) is a common security measure to reduce exposure to automated attacks. Below are the detailed steps to update the SSH configuration and adjust firewall rules accordingly.

---

## 🟦 Step 1: Edit the SSHD Configuration File

Open the SSH server configuration file in a text editor such as **vim**:

```bash
vim /etc/ssh/sshd_config
```

Locate the following line and change the port number (e.g., to **2222**):

```bash
Port 2200
```

> ⚠️ **Note:** Ensure this line is **not commented out** (remove the `#` at the beginning).

---

## 🟩 Step 2: Update The Firewall Rules (Using firewalld)

## ➕ Add The New SSH Port to Firewall

Run the following command to permanently allow TCP traffic on port **2222**:

```bash
firewall-cmd --permanent --add-port=2200/tcp
```

---

## ➖ Remove The Default Port (Optional)

If you want to disallow SSH on the default port **22**, you can remove it:

```bash
firewall-cmd --permanent --remove-service=ssh
```

> This step is optional but recommended for security if you're confident in accessing the system through the new port only.

---

## 🔄 Step 5:  Reload The Firewall Rules

After making changes, reload **firewalld** to apply them:

```bash
firewall-cmd --reload
```

---


## ✅ Verify The New Port Is Allowed

### Check that port **2200** is now listed:

```bash
firewall-cmd --list-ports
```

### You should see:

```
2200/tcp
```

---

## 🔄 Step 3: Restart Services

Restart the SSH service to load the updated configuration:

```bash
systemctl restart sshd.service
```

---

## 🔍 Step 4: Verify SSH Service Port

### Check that the SSH daemon is listening on the new port:

```bash
netstat -nltup | grep sshd
```

### Alternatively, if `netstat` is unavailable:

```bash
ss -nltup | grep sshd
```

---

## 🔗 Step 5: Connect Using The New SSH Port

### 🔗 Connect to the Server on the New SSH Port

```bash
ssh -p 2200 armour@192.168.1.21
```

```bash
ssh armour@192.168.1.21 -p 2200
```

Replace **root** and **192.168.1.32** with the appropriate username and server IP address.

---

## 📝 Example SSH Configuration Snippet (`/etc/ssh/sshd_config`)

Here’s a sample snippet of the modified **sshd_config** with the relevant `Port` directive and other useful defaults:

```bash
Include /etc/ssh/sshd_config.d/*.conf
Port 2200
ListenAddress 192.168.1.21
AuthorizedKeysFile .ssh/authorized_keys
Subsystem sftp /usr/libexec/openssh/sftp-server
```

> 💡 Make sure to save your changes and always back up configuration files before modifying them.

---

## 🔒 SELinux Consideration (If Enabled)

If SELinux is enabled on your system, you must inform it about the new SSH port:

```bash
semanage port -a -t ssh_port_t -p tcp 2200
```

> You may need to install the **policycoreutils-python** or **policycoreutils-python-utils** package to use `semanage`.



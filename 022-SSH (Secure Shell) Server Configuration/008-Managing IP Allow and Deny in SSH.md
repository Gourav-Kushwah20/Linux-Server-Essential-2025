# 🚫 Managing IP Allow and Deny in SSH

SSH access to a Linux system can be controlled by specifying which users and IP addresses are allowed or denied using directives in the SSH daemon configuration file (`sshd_config`).
This is useful for tightening security, especially on critical systems.

---

## 📂 Step 1: Open SSH Configuration File

Use a text editor like `vim` to open the SSH daemon configuration file:

```bash
vim /etc/ssh/sshd_config
```

---

## 🚷 Step 2: Deny SSH Access From Specific Users or IPs

To block access from specific users or IP addresses, add **DenyUsers** directives to the `sshd_config` file.

```bash
DenyUsers root@192.168.1.7
DenyUsers *@192.168.1.151
DenyUsers *@192.168.1.*
DenyUsers root@*
```

---

## 📘 Description

* **root@192.168.1.7** → Deny SSH login to root from this specific IP.
* **`*@192.168.1.151`** → Deny any user from IP `192.168.1.151`.
* **`*@192.168.1.*`** → Deny any user from any host in the `192.168.1.x` range.
* **`root@*`** → Deny root login from all IP addresses.

---

## 🔄 Step 3: Restart SSH Service

After saving the file, apply the changes by restarting the SSH service:

```bash
systemctl restart sshd.service
```

---
Here is the image content rewritten cleanly in **Markdown format with emojis**:

---

## ✅ Step 4: Allow SSH Access From Specific Users or IPs

If you want to permit access only to selected users or IPs, use the **AllowUsers** directive.
Open the SSH configuration file:

```bash
vim /etc/ssh/sshd_config
```

---

## ➕ Add the following lines to explicitly allow access:

```bash
AllowUsers root@192.168.1.7
AllowUsers armour@*
AllowUsers *@192.168.1.7
```

### 📘 Description

* **root@192.168.1.7** → Allow root login from IP `192.168.1.7`.
* **armour@*** → Allow the user **armour** to log in from any IP.
* **`*@192.168.1.7`** → Allow any user from IP `192.168.1.7`.

> ⚠️ If both **DenyUsers** and **AllowUsers** are present, **DenyUsers takes precedence.**

---

## 🔄 Step 5: Restart SSH Service Again

Finally, restart the SSH service to apply the updated configuration:

```bash
systemctl restart sshd.service
```

---

## 📝 Notes

* ✔️ Always test configuration changes in a second SSH session before restarting the service.
* 🔧 Use **Match** blocks for more granular controls per user, group, or network.
* 🔥 Consider combining this with firewall rules for stronger access restrictions.

---

# 🎯 Bind SSH To A Specific IP Address

## 🔒 Bind SSH To A Specific IP Address

By default, the SSH daemon (**sshd**) listens on all available network interfaces. You can restrict it to a specific IP address using the **ListenAddress** directive in the SSH configuration. This can improve security in multi-interface environments or when isolating services by IP.

---

## 📝 Edit SSH Configuration File

### Open the SSH server configuration file:

```bash
vim /etc/ssh/sshd_config
```

### Look for the `ListenAddress` directive and set it to the desired IP address, for example:

```bash
ListenAddress 192.168.1.23
```

### If you're also changing the port (e.g., from 22 to 2222), ensure the `Port` directive reflects that:

```bash
Port 2200
```

> 💡 *You can specify multiple `ListenAddress` entries if the server should listen on more than one IP.*

---

## 🔄 Step 2: Restart The SSH Service

After modifying the configuration file, restart the SSH service to apply the changes:

```bash
systemctl restart sshd.service
```

---

## 🔍 Step 3: Verify SSH Is Listening On The Correct IP And Port

Use `netstat` (or `ss`) to confirm that **sshd** is bound to the specified IP and port:

```bash
netstat -nltup | grep sshd
```

Expected output should show something like:

```
tcp   0   0 192.168.1.38:2200   0.0.0.0:*   LISTEN   <pid>/sshd
```

---

## 📘 Sample SSH Configuration Snippet

Here is a practical snippet from **/etc/ssh/sshd_config** demonstrating both port and IP binding:

```bash
Include /etc/ssh/sshd_config.d/*.conf
Port 2200
ListenAddress 192.168.1.23
AuthorizedKeysFile .ssh/authorized_keys
Subsystem   sftp   /usr/libexec/openssh/sftp-server
```

> 💡 *You can comment out the `#ListenAddress ::` or `#ListenAddress 0.0.0.0` lines if you don’t want the daemon to listen on all interfaces.*

---


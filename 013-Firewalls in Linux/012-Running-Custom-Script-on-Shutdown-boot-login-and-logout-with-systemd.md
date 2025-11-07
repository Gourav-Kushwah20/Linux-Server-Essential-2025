# 🧠 Running Custom Scripts on Shutdown, Boot, Login, and Logout with Systemd

Systemd allows you to hook into system events (boot, shutdown, login, logout) to run your own scripts.
This guide walks through creating scripts and linking them with `systemd` units.

---

## 1. Create Sample Scripts

```bash
vim /opt/shutdown_notify.sh
```

```bash
#!/bin/bash
echo "System shutting down at $(date)" >> /opt/shutdown.log
```

---

```bash
vim /opt/poweron_notify.sh
```

```bash
#!/bin/bash
echo "System started at $(date)" >> /opt/poweron.log
```

---

**Make them executable:**

```bash
chmod +x /opt/shutdown_notify.sh
chmod +x /opt/poweron_notify.sh
```

---

## 2. Create Systemd Services

### 🧩 a) Run Script Before Shutdown

```bash
vim /etc/systemd/system/run-before-shutdown.service
```

```ini
[Unit]
Description=Run custom script before shutdown
DefaultDependencies=no
Before=shutdown.target

[Service]
Type=oneshot
ExecStart=/opt/shutdown_notify.sh
TimeoutStartSec=0

[Install]
WantedBy=shutdown.target
```
- `DefaultDependencies=no` -> ensure it runs even late in shutdown.
- `Before=shutdown-target` -> gurantees it runs before shutdown finalizes.

---

## 🖥️ Run Script at Boot (After Network)

```bash
vim /etc/systemd/system/run-after-poweron.service
```

```ini
[Unit]
Description=Run custom script at boot
After=network.target

[Service]
Type=oneshot
ExecStart=/opt/poweron_notify.sh

[Install]
WantedBy=multi-user.target
```

* Runs **once at boot**, after the network is ready.
* Use **multi-user.target** for non-graphical systems (or **graphical.target** if you need GUI up).

---

## 👤 Run Script After User Login (Graphical)

```bash
vim /etc/systemd/system/run-after-user-login.service
```

```ini
[Unit]
Description=Run custom script after graphical login
After=graphical.target

[Service]
Type=oneshot
ExecStart=/opt/poweron_notify.sh

[Install]
WantedBy=graphical.target
```

* Runs when the **graphical environment** is available.
* For **per-user login scripts**, prefer user-level systemd (see below).

---

## ⚙️ 3. Enable the Services

```bash
systemctl daemon-reload 
```

```bash
systemctl enable run-before-shutdown.service
```

```bash
systemctl enable run-after-poweron.service
```

```bash
systemctl enable run-after-user-login.service
```
reboot to test.

---
## 🧩 4. Run a Script on User Logout

For **shell logout** (e.g., SSH or terminal session):

```bash
~/.bash_logout
```

```bash
#!/bin/bash
echo "User $(whoami) logged out at $(date)" >> /opt/logout.log
```

**Make it executable:**

```bash
chmod +x ~/.bash_logout
```

> **Note:**
> `.bash_logout` only works for **interactive shells**, not GUI logouts.

---

## 👤 5. User-Level Systemd (Alternative Approach)

Instead of system-wide units, you can create **user services**:

**Place files in:**

```bash
~/.config/systemd/user/
```

**Example:**

```bash
~/.config/systemd/user/run-at-login.service
```

```ini
[Unit]
Description=Run script at user login

[Service]
Type=oneshot
ExecStart=/opt/poweron_notify.sh
```

**Enable it:**

```bash
systemctl --user enable run-at-login.service
systemctl --user start run-at-login.service
```

---

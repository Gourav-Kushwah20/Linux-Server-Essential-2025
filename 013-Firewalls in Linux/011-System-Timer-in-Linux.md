# ⏰ Systemd-Timers-in-Linux

## ⚙️ Systemd Timers in Linux
Systemd timers are a modern replacement for traditional `cron jobs` in Linux. They are used to schedule tasks to run at specific times or intervals using `systemd`, which is the default init system in many Linux distributions.

---

## 1️⃣ Overview of Systemd Timers

A systemd timer consists of two main components:

1. 🕒 **Timer Unit (`.timer`)** – Defines when the job should run.  
2. 🧰 **Service Unit (`.service`)** – Defines what should be executed.

✨ Timers provide more flexibility than cron, such as:

- 📝 **Better logging and tracking via [`journalctl`](https://www.freedesktop.org/software/systemd/man/journalctl.html)**
- ⚡ **Ability to run jobs immediately after boot**
- 🎲 **Randomized delays to avoid system overload**

---

## 2️⃣ Creating a Systemd Timer

### 🛠️ Step 1: Create a Service Unit

A service unit defines the actual task that will be executed.

📄 **Example:**
```bash
vim /etc/systemd/system/myjob.service
```
```ini
[Unit]
Description=My Scheduled Job

[Service]
Type=oneshot
ExecStart=/usr/bin/bash -c "echo 'Hello, world!' >> /tmp/hello.txt"
```
💡` Type=oneshot` means the task runs once and exits.

```bash
systemctl start myjob.service 
```
---

### 🛠️ Step 2: Create a Timer Unit

The Timer unit specifies when the service runs.

📄 **Example:**
```bash
vim /etc/systemd/system/myjob.timer
```
```ini
[Unit]
Description=Run My Scheduled Job Every 5 Minutes

[Timer]
OnBootSec=1min
OnUnitActiveSec=5min
Unit=myjob.service

[Install]
WantedBy=timers.target
```
- `Type=oneshot` -> run 1 minutes after boot.
- `OnUnitActiveSec=5min` -> repeat every 5 minutes
- `Unit=myjob.service` -> links the timer to the service

- `enable --now` -> enable and starts the timer immediately.
--

### 🛠️ Step 3:Managing Systemd Timers

**Enable and Start**

```bash
systemctl daemon-reload 
```
```bash
systemctl enable myjob.timer --now
```
- `Daemon-reload` -> reloads systemd configs
- `enable --now` -> enables and starts the timer immediately.

```bash
reboot 
```
- After one min to check `Hello, world!` printed
```bash
cat /tmp/hello.txt
```

### Check Timer Status
```bash
systemctl list-timers -all
```
```bash
systemctl status myjob.service
```

### Manually Run The Job
```bash
systemctl start myjob.service
```
### Disable and Remove

```bash
Systemctl stop myjob.timer
```

```bash
Systemctl disable myjob.timer
```

```bash
rm /etc/systemd/system/myjob.{timer,service}
```

```bash
Systemctl daemon-reload
```

---
## 4. Alternative Scheduling Options

Instead of `OnUnitActiveSec`, you can use **calendar expressions** with `OnCalendar`.

### Examples:

- **Run daily at 3:15 AM**
  ```ini
  OnCalendar=*-*-* 03:15:00
  ```

* **Run every Monday at noon**

  ```ini
  OnCalendar=Mon *-*-* 12:00:00
  ```

* **Run hourly**

  ```ini
  OnCalendar=hourly
  ```

* **Run weekly**

  ```ini
  OnCalendar=weekly
  ```

👉 **You can test your calendar syntax:**

```bash
systemd-analyze calendar "Mon *-*-* 12:00:00"
```

---

## Persistent Execution

```ini
[Timer]
OnCalendar=*-*-* 03:15:00
Persistent=true
```

**`Persistent=true`** ensures missed jobs (e.g., when the system was off) run immediately after boot.

---
## 5. Viewing Logs

- check service logs:
```bash
journalctl -u myjob.service --no-pager
```
- Check timer activity:
```bash
journalctl -u myjob.timer --no-pager
````

---

## 6. Systemd Timers vs Cron

| Feature          | Cron Jobs       | Systemd Timers                                  |
| ---------------- | --------------- | ----------------------------------------------- |
| Logging          | Limited         | Full logs via `journalctl`                      |
| Start at Boot    | No              | Yes (with `OnBootSec`)                          |
| Flexibility      | Fixed intervals | Randomized delays, calendar events, persistence |
| Failure Handling | Manual retry    | Built-in retry options                          |
| Integration      | Independent     | Fully integrated with `systemd`                 |

✅ **Systemd timers** are more powerful, flexible, and reliable, making them the preferred choice in modern Linux environments.

---

## 7. Best Practices

* Store custom timers in `/etc/systemd/system/` (system-wide) or `~/.config/systemd/user/` (per-user).
* Use **descriptive names** for `.service` and `.timer` files.
* Use `Persistent=true` for critical jobs.
* Always check with `systemctl list-timers` to confirm schedules.
* Use `RandomizedDelaySec` for staggered jobs on multiple servers.

```

Let me know if you'd like this exported to a file format like `.md` or `.txt`.
```

# Cron Jobs in Linux ⏰

A **cron job** is a scheduled task in Linux or Unix-like systems that automates processes to run at specified intervals or times. It is managed by the **cron daemon** (`crond`), which continuously runs in the background and checks for tasks defined in the crontab (cron table).

---

## Checking if Cron is Installed 🔍

Before using cron, ensure it is installed:

```bash
rpm -qa | grep cron
```
```bash
rpm -qa | grep crontabs
```

If not installed:

```bash
yum install crontabs
```

Verify installation:

```bash
rpm -ql crontabs
```
```bash
rpm -qi crontabs
```

---

## Managing Cron Service ⚙️

Enable and start the cron service:

```bash
systemctl enable crond
```
```bash
systemctl start crond
```
```bash
systemctl status crond
```

---

## Cron Directories 📂

Cron jobs are managed in specific directories:

```bash
ls -lh  /etc/ | grep cron
```

```bash
ls -1 /etc/cron*
```
You'll typcially see:
```bash
/etc/cron.daily
/etc/cron.hourly
/etc/cron.monthly
/etc/cron.weekly
/etc/crontab
/etc/sysconfig/run-parts
/usr/bin/run-parts
/usr/share/licenses/crontabs
/usr/share/licenses/crontabs/COPYING
/usr/share/man/man4/crontabs.4.gz
/usr/share/man/man4/run-parts.4.gz
```
- **System-wide cron directories** (for all users):
  - `/etc/crontab`: System-wide crontab file.
  - `/etc/cron.d/`: Directory for additional cron jobs.

- **User-specific cron directories**:
  - `crontab -e`: Edit the current user's crontab.
  - `crontab -l`: List the current user's cron jobs.
  - `crontab -r`: Remove the current user's crontab.

---

## 🗂️ System-Wide Cron Directories

These are controlled by `run-parts`, which executes all scripts inside a directory.

| Directory                                   | Frequency           |
|----------------------------------------------|---------------------|
| [🕐 /etc/cron.hourly](#1-etccronhourly-)     | Runs every hour     |
| [📅 /etc/cron.daily](#2-etccrondaily-)       | Runs once per day   |
| [📆 /etc/cron.weekly](#3-etccronweekly-)     | Runs once per week  |
| [🗓️ /etc/cron.monthly](#4-etccronmonthly-)  | Runs once per month |

---

## 1. /etc/cron.hourly 🕐

- This directory contains scripts that run **once every hour**.
- The execution time is controlled by `/etc/crontab` or system cron settings.
- Any executable script placed in this directory runs automatically each hour.

**Example: A script to sync logs every hour**

```bash
mkdir -p /backup/logs/
```

```bash
vim /etc/cron.hourly/log_sync
```
```bash
#!/bin/bash
rsync -av /var/log/ /backup/logs/
```

Save this script as `/etc/cron.hourly/log_sync` and make it executable:

```bash
chmod +x /etc/cron.hourly/log_sync
```

---

## 2. /etc/cron.daily 📅

- This directory contains scripts that run **once per day**.
- The default execution time is usually **3:00 AM**, but it can vary by system.
- Common tasks include system updates, log rotation, and database backups.

**Example: A script to clean temporary files daily**

```bash
vim /etc/cron.daily/clean_tmp
```
```bash
#!/bin/bash
find /tmp -type f -mtime +7 -delete
```

Save this script as `/etc/cron.daily/clean_tmp` and make it executable:

```bash
chmod +x /etc/cron.daily/clean_tmp
```

---

## 3. /etc/cron.weekly 📆

- Scripts in this directory run **once per week**.
- The default execution time is usually **Sunday at 4:00 AM**, but this depends on system settings.
- Weekly tasks may include full system backups, package updates, or maintenance scripts.

**Example: A script to create a weekly system backup**

```bash
vim /etc/cron.weekly/system_backup
```
```bash
#!/bin/bash
tar -czf /backup/system_backup_$(date +\%F).tar.gz /home /etc /var
```

Save this script as `/etc/cron.weekly/system_backup` and make it executable:

```bash
chmod +x /etc/cron.weekly/system_backup
```

---

## 4. /etc/cron.monthly 🗓️

- Scripts in this directory run **once per month**.
- The default execution time is usually the **first day of the month at 5:00 AM**.
- Suitable for long-term maintenance tasks such as database cleanup, report generation, or software updates.

**Example: A script to generate a monthly user activity report**

```bash
vim /etc/cron.monthly/user_report
```
```bash
#!/bin/bash
cat /var/log/secure | grep password > /backup/user_activity_$(date +\%Y-\%m).log
```

Save this script as `/etc/cron.monthly/user_report` and make it executable:

```bash
chmod +x /etc/cron.monthly/user_report
```
```bash
ls -lh /etc/cron*
```
---


## 🛠️ Manually Running Cron Scripts

You can manually run all scripts in a cron directory using `run-parts`:

```bash
run-parts /etc/cron.hourly
```
```bash
run-parts /etc/cron.daily
```
```bash
run-parts /etc/cron.weekly
```
```bash
run-parts /etc/cron.monthly
```
---

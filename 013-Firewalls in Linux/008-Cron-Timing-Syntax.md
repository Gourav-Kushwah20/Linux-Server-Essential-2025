## Viewing and Editing Crontab 📝

- To list the configuration files for crontabs:
  ```bash
  rpm -qc crontabs
  ```
- To edit the system crontab file:
  ```bash
  vim /etc/crontab
  ```

  ```bash
  SHELL=/bin/bash
  PATH=/sbin:/bin:/usr/sbin:/usr/bin
  MAILTO=root

  # For details see man 4 crontabs

  # Example of job definition:
  # .---------------- minute (0 - 59)
  # |  .------------- hour (0 - 23)
  # |  |  .---------- day of month (1 - 31)
  # |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
  # |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
  # |  |  |  |  |
  # *  *  *  *  * user-name  command to be executed
   */2 * * * * root rm -rf /tmp/*
  ```

- For manual reference:
  ```bash
  man crontab
  ```
- Online crontab scheduling reference:  
  [Crontab Guru 🌐](https://crontab.guru/#10_10_*_*_*)

---

## Checking Cron Logs 📖

To debug or check cron execution logs, use:
```bash
cat /var/log/cron
```

---

## Cron Environment 🌎

- **Absolute paths are required** (`/usr/bin/php` not just `php`).
- Set environment variables in crontab:
  ```bash
  SHELL=/bin/bash
  PATH=/sbin:/bin:/usr/sbin:/usr/bin
  MAILTO=root
  ```

---

## Cron Logs & Debugging 🐞

- View logs:
  ```bash
  cat /var/log/cron
  ```
- Monitor in real time:
  ```bash
  tail -f /var/log/cron
  ```

---

## Cron vs. Anacron
- Cron: Skips jobs if the system if off.
- Anacron: Ensure jobs (Daily/weekly/monthly) still run after the system restarts.

---

## Security Considerations

- **cron.all** -> Users allowed to use cron.
- **cron.deny** -> users denied from using cron.
- Located in `/etc/`.

---
## Working with Date and Time in Cron Jobs.

To fetch the current date and time:

```bash
date
```
Formatting date output:

```bash
date "+%F"
```
```bash
date "+%F-%H-%M"
```
```bash
date "+%d-%m-%Y-%H-%M"
```
```bash
date "+%H-%M-%d-%m-%Y"
```

Creating Files with timestamped name:
```bash
touch `date "+%d-%m-%Y-%H-%M"`
```
```bash
touch `date "+%d-%m-%Y-%H-%M-%S"`
```
Archiving files with timestamps:

```bash
tar -cvf /tmp/`date "+%d-%m-%y-%H-%M"`.tar /home/armour
```

```bash
tar -cvf /tmp/`date "+\%d-\%m-\%y-\%H-\%M"`.tar /home/armour
```

## Viewing & Editing Crontab
- List current user's cron jobs:
```bash
crontab -l
```
- Edit current user's crontab:
```bash
crontab -e
```
- Edit another user's crontab:
```bash
crontab -e -u username
```
- Remove all jobs:
```bash
crontab -r
```

```bash
crontab -e -u sec-learner
```

## Common Scheduling Examples
| Schedule | Cron Syntax | Description |
|----------|------------|-------------|
| Every minute | `* * * * *` | Runs every minute |
| Every hour | `0 * * * *` | Runs every hour |
| Every day at 2 AM | `0 2 * * *` | Runs at 2 AM daily |
| Every Monday at 5 PM | `0 17 * * 1` | Runs every Monday at 5 PM |
| Every 10 minutes | `*/10 * * * *` | Runs every 10 minutes |
| Every 6 hours | `0 */6 * * *` | Runs every 6 hours |

---

## Cron Job Examples & Explanations

---

## 1. Every Minute

```cron
* * * * * command
````

* Runs every minute of every hour, every day.

Example:

```cron
* * * * * echo "This runs every minute" >> /tmp/cron_log.txt
```

---

## 2. Every Minute from 1 Through 10

```cron
1-10 * * * * command
```

* Runs every minute from **1 through 10** of every hour.

Example:

```cron
1-10 * * * * echo "Running during first 10 minutes of every hour" >> /tmp/cron_log.txt
```

---

## 3. Every 2nd Hour at Minute 5

```cron
5 0-23/2 * * * command
```

* Runs at minute **5** past every **2nd hour** (i.e., 12:05 AM, 2:05 AM, 4:05 AM, ...).

Example:

```cron
5 0-23/2 * * * echo "Running at 5 minutes past every even hour" >> /tmp/cron_log.txt
```

---

## 4. At Specific Minutes

```cron
1,10,15,35 * * * * command
```

* Runs at minute **1, 10, 15,** and **35** of every hour.

Example:

```cron
1,10,15,35 * * * * echo "This runs at specified minutes" >> /tmp/cron_log.txt
```

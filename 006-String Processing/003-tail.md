## 🔚 `tail` Commands

The `tail` command is used to display the **last part** of files, typically the last 10 lines by default. It's useful for monitoring log files and viewing recent file changes.

---

### 📄 Basic Usage

#### Shows the last 10 lines of `/etc/passwd.`
```bash
tail /etc/passwd
```
#### Displays the last 10 lines of the system log file

```bash
tail /var/log/messages
```
### 🔢 View a Specific Number of Lines
```bash
tail -n 40 /var/log/messages
```
Shows the last 40 lines of `/var/log/messages.`

```bash
tail -n 12 /var/log/messages /etc/passwd /etc/shadow /etc/gshadow
```
Displays the last 12 lines from each of the listed files.

## 🔍 Verbose Output
```bash
tail -v -n 3 /var/log/messages
```
Shows the last 3 lines of the file with the filename header included.
## 🔠 Output by Character Count

```bash
tail -c 12  /etc/passwd /etc/shadow /etc/gshadow /etc/group
```
Displays the last 12 characters from each specified file.

## 📡 Follow File Updates in Real Time
```bash
tail -f  /var/log/messages
```
Continuously monitors `/var/log/messages` for new lines being appended.
```bash
tail -f /var/log/httpd/access_log
```
Live monitor web server access logs (Apache in this case).

####  Use tail to debug logs, monitor system activity, or view recent file content as it updates.
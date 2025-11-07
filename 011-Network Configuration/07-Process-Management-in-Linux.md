# **Process Management** in Linux
Process management in linux involves monitoring, controlling and terminating processes to ensure system stability and performance. Below are the most commonly used commands and thier usage.

## Running Commands in `Background` & `Foreground`

- Run in Foreground:
```bash
ping 8.8.8.8
```
- Run in background:
```bash
ping 8.8.8.8 &
```
- Send output to /dev/null and run in background:

```bash
ping 8.8.8.8 > /dev/null &
```

```bash
ps -aux | grep ping
```

```bash
seq 100000000000000000000000 > /dev/null &
```
## Managing Jobs
- list background jobs:
```bash
jobs
``` 
- bring the last job to the foreground:
```bash
fg
```
- bring job number 2 to the foreground:
```bash
fg %2
```

---

## Viewing Process with `ps`
- Current shell processes:
```bash
ps
```
- Detailed process info:
```bash
ps -l
```
- All processes with CPU/memory usage (BSD style):
```bash 
ps aux
```
- All Process (Standard style):
```bash
ps -e
```
- Tree view of processes:
```bash
ps -axjf
```
- Processes owned by **root**:
```bash
ps -U root -u root u
```
- Processes Owned by user **armour**:
```bash
ps -U armour -u armour u
```
---
## Monitoring Processes in Real-Time
- View real time runnning processes:
```bash
top
```
- Install **htop** (modern alternative):

#### Debain/Ubuntu
```bash
apt install htop
```
#### RHEL/CentOS
```bash
yum install htop
```
- Interactive process monitor:
```bash
htop
```
- Show tree structure:
```bash
htop -t
```
- Show Processes for a Specific user:
```bash
htop -u armour
```

- Monitor a specific PID:
```bash
htop -p 2780
```
---
## Killing Processes
- check `seq` processes `id`: 
```bash
ps -aux | grep seq
```
- Kill by PID:
```bash
kill 2526
```
- Force kill(SIGKILL):
```bash
kill -9 1891
```
---
## Kill all Instance of a commands:
```bash
yum install psmisc
```
```bash
killall seq
```
```bash
killall gdm
```
- Kill processes using regex pattern:
```bash
killall -r firefox
```
- kill processes owned by user **armour**:
```bash
killall -u armour
```
- kill by process name:
```bash
pkill ping
```
- kill all processes for a user:
```bash
pkill -u armour
```
- kill exact process name awned by a user:
```bash
pkill -u armour -x ping
```
---
## Summary table of Process Management:
| Command                    | Purpose                                            | Example                  |
| -------------------------- | -------------------------------------------------- | ------------------------ |
| `ps aux`                   | Show all running processes                         | `ps aux \| grep firefox` |
| `ps -ef`                   | Full-format process listing                        | `ps -ef`                 |
| `top`                      | Real-time process monitoring                       | `top`                    |
| `htop`                     | Interactive process viewer (better than `top`)     | `htop`                   |
| `pgrep <name>`             | Find PID of a process                              | `pgrep sshd`             |
| `kill <PID>`               | Kill a process by PID                              | `kill 1234`              |
| `kill -9 <PID>`            | Force kill a process                               | `kill -9 1234`           |
| `pkill <name>`             | Kill by process name                               | `pkill firefox`          |
| `killall <name>`           | Kill all processes with that name                  | `killall firefox`        |
| `renice -n <val> -p <PID>` | Change priority of a running process               | `renice -n -5 -p 1234`   |
| `<cmd> &`                  | Run a process in background                        | `firefox &`              |
| `jobs`                     | List background jobs                               | `jobs`                   |
| `fg %<job>`                | Bring background job to foreground                 | `fg %1`                  |
| `bg %<job>`                | Resume a stopped job in background                 | `bg %1`                  |
| `uptime`                   | Show load average                                  | `uptime`                 |
| `vmstat`                   | CPU, memory, I/O stats                             | `vmstat 2`               |
| `pidstat`                  | Per-process resource stats                         | `pidstat -p 1234`        |

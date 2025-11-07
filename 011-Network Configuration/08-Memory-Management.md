# **Memory-Management**-in-linux
Memory management is crucial for monitoring system performanace and turbleshooting issues. Below are essential commands for managing and analyzing memory usage in linux.

## Check Memory Usage(RAM) with `free`

The free command display available and used memory in the system.

- Shows memory usage in a **human-readable format**:
```bash
free -h
```
- Display memory usage in **gigabytes**:
```bash
free -g
```
- Display memory usage in **megabytes**:
```bash
free -m 
```
- Displays memory usage in **kilobytes**:
```bash
free -k
```
---
## Monitoring System Performance with `vmstat`
The **vmstat** commands provides details system performance statics,Includes CPU memory,and Disk usage.

- Display CPU memory and Usage statics:
```bash
vmstat
```
- Shows active and inactive memory:
```bash
vmstat -a
```
- Displays disk Statistics:
```bash
vmstat -d
```
- Shows statistics in kilobytes:
```bash
vmstat -S k
```
- Show statistics in megabytes:
```bash
vmstat -S M
```
- Continuous monitoring: 5 reports every 4 seconds:
```bash
vmstat 4 5
```
- Same as above but in megabytes:
```bash
vmstat -S M 4 5
```
- Includes timestamp in output(megabytes with time):
```bash
vmstat -t M 4 5 M
```
---
## Disk and CPU Performance with iostat
The **iostat** command help analyze CPU and disk I/O performance.

- Install **iostat** if not available:
```bash
yum install sysstat
```
- Display CPU and disk performance statistics:
```bash
iostat
```
- Update the statistics 5 time at 4 - second intervals:
```bash
iostat 4 5
```

---
## Listing Open Files with `lsof`
The **lsof** (list Open Files) commands displays files opened by processes.

- Install `lsof` if not available:
```bash
yum install lsof
```
### Basic Usage
- lists all open files:
```bash
lsof
```
- Shows opne files for users root:
```bash
lsof -u root
```
- Shows open files for armour:
```bash
lsof -u armour
```
- Displays open files for root, without resolving hostnames:
```bash
lsof -u root -n
```

### Checking Network Connections
- Lists all open network connections:
```bash
lsof -i
```
- Same as above, but without resolving hostname:
```bash
lsof -i -n
```
- Lists open TCP connections:
```bash
lsof -n -i TCP
```
- Lists open UDP connections:
```bash
lsof -n -i UDP
```
- Shows processes using port 22(SSH):
```bash
lsof -n -i TCP:22
```
- Shows processes using port 80(HTTP),443(HTTPS),22(SSH):
```bash
lsof -n -i TCP:80,443,22
```
- Display TCP connections within port range 80-500:
```bash
lsof -n -i TCP:80-500
```
- Shows UDP Connections on port 53(DNS):
```bash
lsof -n -i UDP:53
```

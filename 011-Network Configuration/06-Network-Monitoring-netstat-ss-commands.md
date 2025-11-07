# Network-Monitoring-`netstat`-`ss`-Commands

NetStat and ss are powerful tools to monitor network connection and sockets on linux.

- While `netstat` is older,part of the **net-tools** packages and often requires installation by default.
- `ss`(sockets statistics) is its modern replacement :faster, more efficient,and pre-installed on most Linux distributions.

### Installation netstat (if not available)
- CENTOS
```bash
yum install net-tools
```

- Debain
```bash
apt install net-tools
```
- The `ss` Command is Usually available out of the box on modern linux system.

---

## netstat Usage:

- Display active connection:
```bash
netstat
```
- Show all sockets (listening  + established)
```bash
netstat -a
```
- Show numerical address(skip DNS lookup):
```bash
netstat -na
```
- Show only TCP connection:
```bash
netstat -t
```
- Show only UDP connection:
```bash
netstat -u
```
- Show both TCP and UDP connection:
```bash 
netstat -tu
```
- Show both TCP and UDP in numeric format (faster) connection:
```bash
netstat -tun
```
- Show both TCP and UDP ports:
```bash
netstat -ltun
```
- Show listening service with numeric IPs and process details:
```bash
netstat -nltup
```
```bash
netstat -natup
```
---

## `SS` Commands (Usage Modern Alternative):

- Display active connections:
```bash
ss
```
- Show only listening sockets:
```bash
ss -l
```
- Show all sockets (listening + active):
```bash
ss -a
```
- show numerical address only:
```bash
ss -na
```
- Show only tcp connections:
```bash
ss -t
```
- Show only UDP connections:
```bash
ss -u
```
- Show both TCP and UDP connections:
```bash
ss -tu
```
- Show both TCP and UDP numeric format:
```bash
ss -tun
```
- Show listening ports for TCP and UDP:
```bash
ss -nltu
```
- Show listening service with numeric IPs and process details:
```bash
ss -nltup
```
```bash
ss -natup
```

---
## `netstat` and  `ss`
| Feature                                    | `netstat` 🕰️          | `ss` ⚡               |
| ------------------------------------------ | ---------------------- | -------------------- |
| TCP/UDP connection info                    | ✅                      | ✅                    |
| Listening ports                            | ✅                      | ✅                    |
| Routing tables                             | ✅                      | ❌ (use `ip route`)   |
| Interface stats                            | ✅                      | ❌ (use `ip -s link`) |
| Socket details (queue sizes, states, etc.) | ❌ Limited              | ✅ Very detailed      |
| Speed & efficiency                         | ⏳ Slower               | ⚡ Faster             |
| Default availability on modern Linux       | ❌ Not always installed | ✅ Pre-installed      |


## Real-World Usage Example:

- Find which process is using a specific port (e.g port 80)

```bash
ss -ltup | grep :80
```

```bash
ss -ltup | grep :445
```

```bash
ss -ltup | grep :22
```

```bash
netstat -tulnp| grep :80 
```

- Display established Connection only:
```bash
ss -t state established
netstat -ant | grep ESTABLISHED
```

- Show Summary of Socket usage:
```bash
ss -s
```
```bash
netstat -s
```
- find connection to specific ip address

```bash
ss -tn dst 192.168.1.4
```

```bash
netstat -ant | grep 192.168.1.4
```
- Check which user/service are bound to ports
```bash
ss -lptn
```
```bash
netstat -lptn
```
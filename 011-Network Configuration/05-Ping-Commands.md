# **Ping** Commands :
The ping Commands send ICMP echo Request packet to verify connectivitity to a host.

- Continous ping:
```bash
ping 8.8.8.8
```

- Send 2 packets:
```bash
ping 8.8.8.8 -c 2
```

- Send a Large packet (65,500 bytes,default = 56 bytes):
```bash
ping 192.168.1.1 -s 65500
```
- Ping from Specific interfaces:
```bash
ping -I enp0s3 192.168.1.1 -s 65500 -c 4
```

- Add a custom Payload (hexa decimal 42):

```bash
ping 192.168.1.1 -c 2 -s 65500 -I enp0s3 -p 42
```
- Set Interval (2 packet, every 10s):
```bash
ping 192.168.1.1 -i 10 -c 2
```
```bash
ping 192.168.1.1 -i 10 -c 2 -p 42
```

- Resolve and ping a Hostname:
```bash
ping armourinfosec.com
```
- IPv5 ping a Hostname:
```bash
ping -6 armourinfosec.com
```
- Send flood pings (as fast as Possible,max packtes size):
```bash
ping 192.168.1.1 -f -s 65500
```
---

## Traceroute
`Traceroute` maps the path packets take to reach a destination.

### Installation:
- On CentOS:
```bash
yum install traceroute -y
```

- On Debain:
```bash
apt install traceroute -y
```
### Usage:
- Trace route to Google DNS:

```bash
traceroute 8.8.8.8
```

- Trace route  to a hostname:
```bash
traceroute armourinfosec.io
```
### TCP Traceroute
- Useful when ICMP is Blocked (requires tcptraceroute packeges): 

```bash
tcptraceroute 8.8.8.8
```

### Tracepath Command
Similar to traceroute but does not require root privileges.

- Installation RHEL / CentOS

```bash
yum install iputils iputils-tracepath
```

- Installation Debain/Ubuntu
```bash
apt install iputils-tracepath
```

- Trace path to google DNS:
```bash

```
# 🔥Firewall - Network Security Barrier

A **firewall** is a security device or software that monitors and controls incoming and outgoing network traffic. It acts as a protective barrier between trusted internal networks and untrusted external networks (like the internet). 🌐

The primary function of a firewall is to allow or deny network traffic based on a set of rules. 

## Key Functions of a Firewall

- 🚦 **Traffic Filtering:** Examines packets and applies security rules based on IP, ports, and protocols.
  - **Example:** A rule may allow HTTP traffic (port 80) but block FTP traffic (port 21).

- 🔐 **Access Control:** Controls who can access specific parts of a network.
  - **Example:** Restrict SSH access (port 22) to only internal IPs.

- 🚨 **Intrusion Prevention:** Detects suspicious activity (e.g., port scanning, large data transfers). Can block threats automatically before they exploit vulnerabilities.

- 📝 **Monitoring & Logging:** Keeps logs of traffic activity for auditing and forensic analysis. Helps in incident response and identifying security threats.

---
## Types of Firewalls

Firewalls can be categorized based on how they inspect traffic and their network placement.

### 1. Packet-Filtering Firewalls (Stateless)

- Evaluate each packet individually without context of previous packets.
- Operate at the network layer (Layer 3).
- Apply rules based on source/destination IP, port, and protocol.
- Fast processing, lightweight, and simple to configure.
- Less secure; cannot detect complex attacks like replay attacks or fragmentation.

**Example Rule:**
`Allow TCP traffic from 192.168.1.100 to 10.0.0.5 on port 80.`

---
## 2. Stateful Firewalls

- Track the state of active connections. 🤝
- Operate at the network and transport layers (Layer 3 & 4).
- Allow packets only if part of an established or related session.
- More secure, can prevent SYN floods and spoofing.
- Use more resources and slower than stateless firewalls.

**Example:**
Allow new TCP connections for SSH and track established sessions.

---
### 3. Next-Generation Firewalls (NGFW)

- Advanced firewalls with deep packet inspection (Layer 7). 🕵️
- Include Intrusion Prevention System (IPS), malware filtering, and VPN capabilities.
- Can detect and block complex threats like SQL Injection and Cross-site scripting (XSS).

---
### Comparison: Stateless vs. Stateful Firewalls

| Feature | Stateless Firewall | Stateful Firewall |
| :--- | :--- | :--- |
| **Tracking** | Does not track connections | Tracks connection states |
| **Security** | Less secure, vulnerable to spoofing | More secure, resistant to attacks |
| **Performance** | Faster (no session tracking) | Slower (tracks sessions) |
| **Use Case** | Simple filtering, high-speed needs | Enterprise security, deeper inspection |
| **Vulnerabilities** | Susceptible to packet spoofing | Resistant to spoofing & replay attacks |

---

## Firewalls in Linux

1. **iptables** (Legacy - Both Stateless and Stateful) 🐧
    - Stateless if only filtering by IP/port/protocol.
    - Stateful if using modules `-m state` or `-m conntrack`.

Here is the content of the image, transcribed into markdown:

### Example stateless rule:

```bash
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

```bash
iptables -P INPUT DROP
```

### Example stateful rule:

```bash
iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
```

```bash
iptables -A INPUT -p tcp --dport 22 -m conntrack --ctstate NEW -j ACCEPT
```

```bash
iptables -P INPUT DROP
```

---

### 2. firewalld (Stateful by Default) 

  * Uses `nftables` or `iptables` backend.
  * Tracks connections and supports dynamic rule management.

**Example:**

```bash
firewall-cmd --permanent --add-service=ssh
```
```bash
firewall-cmd --reload
```
---

### 3. UFW (Uncomplicated Firewall - Stateful by Default) 🛡️

  * Wrapper around `iptables`.
  * Automatically tracks connection states.

**Example:**

```bash
ufw allow 22/tcp
```

```bash
ufw enable
```

**Example to block all except SSH:**

```bash
ufw default deny incoming
```

```bash
ufw default allow outgoing
```

```bash
ufw allow ssh
```

```bash
ufw enable
```
---
### Summary 📋

* **Stateless** = fast but less secure. 💨
* **Stateful** = secure but slower. 🐌
* **NGFW** = advanced protection with deep inspection. 🔍
* **Linux tools**: iptables, firewalld, UFW.🐧
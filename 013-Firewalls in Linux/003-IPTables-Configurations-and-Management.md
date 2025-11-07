# IPTables-Configuration-and-Management ⚙️

## Checking and Managing Firewall Services

### Check if Firewalld is Installed

```bash
rpm -qa | grep firewalld
```

### Get Firewalld Package Information

```bash
rpm -qi firewalld-0.6.3.13.el7_9.noarch
```

### Check Status of IPTables and Firewalld

```bash
systemctl status iptables.service
```
```bash
systemctl status firewalld.service
```

### Disable and Stop Firewalld

```bash
systemctl disable firewalld.service
```
```bash
systemctl mask firewalld.service
```
```bash
systemctl stop firewalld.service
```

### Remove Firewalld

```bash
rpm -qf /usr/lib/systemd/system/firewalld.service
```
```bash
yum remove firewalld
```
---

## Installing Required Packages 📦
  - **Install Netcat**

<!-- end list -->

```bash
yum install netcat
```

  - **Verify Netcat installation**

<!-- end list -->

```bash
rpm -qa | grep netcat
```

  - **Check IPTables Installation**

<!-- end list -->

```bash
rpm -qa | grep iptables
```

  - **Search for IPTables Packages**

<!-- end list -->

```bash
yum search iptables
```

  - **Install IPTables (Legacy + Utilities + Services)**

<!-- end list -->

```bash
yum install iptables-legacy.x86_64 iptables-utils.x86_64 iptables-services.noarch
```

  - **Verify IPTables Installation**

<!-- end list -->

```bash
rpm -qa | grep iptables
```

  - **Get Package Info**

<!-- end list -->

```bash
rpm -qi iptables-legacy-1.8.10.5.1.el9.next.x86_64
```
```bash
rpm -ql iptables-legacy-1.8.10.5.1.el9.next.x86_64
``` 
```bash
rpm -qc iptables-legacy-1.8.10.5.1.el9.next.x86_64
```

---

## ⚙️ Managing IPTables Service

  - **Enable and Start**
    ```bash
    systemctl enable iptables.service
    ```
    ```bash
    systemctl enable ip6tables.service
    ```
    ```bash
    systemctl start iptables.service
    ```
    ```bash
    systemctl start ip6tables.service
    ```
  * **Restart**
    ```bash
    systemctl restart iptables.service
    ```
    ```bash
    systemctl restart ip6tables.service
    ```

---

## 👀 Viewing IPTables Rules

  * **List Rules**
    ```bash
    iptables -L
    ```
    ```bash
    iptables -L -n
    ```
    ```bash
    iptables -n -L INPUT
    ```
    ```bash
    iptables -L OUTPUT
    ```
    ```bash
    iptables -L FORWARD
    ```
  * **List with Line Numbers**
    ```bash
    iptables -n --line-numbers -L
    ```
    ```bash
    iptables -n --line-numbers -L INPUT
    ```
    ```bash
    iptables -n --line-numbers -L OUTPUT
    ```
    ```bash
    iptables -n --line-numbers -L FORWARD
    ```

---

## 📑 IPTables Rules Breakdown

### Rules:

  * **Rule 1:** Allow existing/related connections.
  * **Rule 2:** Allow ICMP (ping, traceroute).
  * **Rule 3:** Allow all protocols ⚠ (risky).
  * **Rule 4:** Allow new TCP connections on SSH (22).
  * **Rule 5:** Reject all other traffic with ICMP "host prohibited".

---

## 📝 Configuring IPTables Rules

- **Allow Specific Ports**

<!-- end list -->

```
iptables -I INPUT 5 -p tcp --dport 21 -j ACCEPT
```

- **Insert Multiple Port Rules**

<!-- end list -->

```bash
iptables -I INPUT 5 -p tcp --dport 80:90 -j ACCEPT
```
```bash
iptables -I INPUT 5 -p tcp --dport telnet -j ACCEPT
```
```bash
iptables -I INPUT 5 -p udp --dport 53 -j ACCEPT
```
```bash
iptables -I INPUT 5 -p udp --dport 67 -j ACCEPT
```
```bash
iptables -I INPUT 5 -p udp --dport 110:130 -j ACCEPT
```

- **Delete a Rule**

<!-- end list -->

```bash
iptables -D INPUT 6
```

- **Block Specific Traffic**

<!-- end list -->

```bash
iptables -I INPUT 4 -p tcp -s 192.168.1.6 --dport 22 -j DROP
```
```bash
iptables -I INPUT 4 -p tcp -s 192.168.1.45 --dport 22 -j REJECT
```
```bash
iptables -I INPUT 5 -p tcp -d 192.168.1.37 --dport 30 -j ACCEPT
```

---

## 🚫 Managing Output Rules

-**Drop ICMP to 8.8.8.8**

    ```bash
    iptables -I OUTPUT 1 -d 8.8.8.8 -p icmp -j DROP
    ```

- **Block HTTP(S) to Specific Domains** 🛑

    ```bash
    iptables -I OUTPUT 1 -d google.com -p tcp --dport 80 -j DROP
    ```

    ```bash
    iptables -I OUTPUT 1 -d www.facebook.com -p tcp --dport 443 -j DROP
    ```
    (⚠ Note: DNS resolution might fail if not using IP directly. Consider using IP addresses for more reliable blocking.)

---

### Saving and Restoring Rules 💾

- **Backup Current Rules**
    ```bash
    cp /etc/sysconfig/iptables /etc/sysconfig/iptables.back
    ```
- **Save & Restart**
    ```bash
    service iptables save
    ```
    ```bash
    service iptables stop
    ```
    ```bash
    service iptables start
    ```
    ```bash
    service iptables restart
    ```
- **Restore After Reboot**
    ```bash
    iptables-restore < /etc/sysconfig/iptables.back
    ```
- **Save Current Rules**
    ```bash
    iptables-save > /etc/sysconfig/iptables
    ```
---
## Ensure IPTables Starts on Boot 💾

```bash
yum install iptables-services
```

```bash
systemctl mask firewalld
```

```bash
systemctl enable iptables
```

```bash
systemctl enable ip6tables
```

```bash
systemctl stop firewalld
```

```bash
systemctl start iptables
```

```bash
systemctl start ip6tables
```

---

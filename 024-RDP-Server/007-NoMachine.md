# **NoMachine 🚀**

### **Install NoMachine as usual 📥**

```bash
wget https://download.nomachine.com/download/8.16/Linux/nomachine_8.16.1_1_x86_64.rpm
```

```bash
yum install ./nomachine_8.16.1_1_x86_64.rpm
```

```bash
rpm -ivh nomachine_7.6.2_4_x86_64.rpm
```

---

### **Check if NoMachine is listening 👀**

```bash
netstat -nltup | grep 4000
```

---

## **Open port 4000 using firewalld 🔥🛡️**

### **Open port 4000/tcp permanently 🔓**

```bash
firewall-cmd --permanent --add-port=4000/tcp
```

### **Open port 4000/udp permanently 🔓**

```bash
firewall-cmd --permanent --add-port=4000/udp
```

---

### **Reload firewalld to apply changes 🔁**

```bash
firewall-cmd --reload
```

---

### **(Optional) Verify the port is open ✔️**

```bash
firewall-cmd --list-ports
```

You should see **4000/tcp** in the list.

---

## 💡 Extra Tip

If you want to **restrict access to port 4000** only for a **specific trusted IP** (for security), you can run:

```bash
firewall-cmd --permanent --add-rich-rule='
  rule family="ipv4"
  source address="192.168.1.100/32"
  port protocol="tcp" port="4000" accept
`
```

Reload firewalld:

```bash
firewall-cmd --reload
```

> 🔄 Replace **192.168.1.100** with your trusted client's IP address.

---

# 🧪 What is `tcpdump`?

**`tcpdump`** is a powerful **command-line packet analyzer** tool used to **capture and inspect network traffic**.

It allows you to:

* 🕵️‍♂️ **Monitor live network traffic**
* 🧱 **Debug network issues**
* 🔒 **Analyze packets for security audits**
* 🧪 **Capture traffic for offline analysis**

---

## 📦 Installation

* On **Debian/Ubuntu**:

  ```bash
  apt install tcpdump
  ```

* On **RHEL/CentOS**:

  ```bash
  yum install tcpdump
  ```

* On **Fedora**:

  ```bash
  dnf install tcpdump
  ```

---
Sure! Here's your `tcpdump` guide written in **Markdown format** with **emojis** for readability and engagement:

---

## 🛠️ `tcpdump` Command Guide

A powerful command-line packet analyzer tool for capturing and inspecting network traffic in real time.

---

## 📘 Basic Commands

* 🆘 **View help and usage options**

  ```bash
  tcpdump -h
  ```

* 📋 **List available interfaces**

  ```bash
  tcpdump -D
  ```

* 🌐 **List all interfaces (alternate)**

  ```bash
  tcpdump --list-interfaces
  ```

---

## 📡 Capturing Packets

* ▶️ **Capture on interface `enp0s3`**

  ```bash
  tcpdump -i enp0s3
  ```

* 📦 **Capture only TCP packets**

  ```bash
  tcpdump -i enp0s3 tcp
  ```

* 📦 **Capture only UDP packets**

  ```bash
  tcpdump -i enp0s3 udp
  ```

* 🚫 **Disable DNS resolution (faster output)**

  ```bash
  tcpdump -n -i enp0s3 tcp
  tcpdump -n -i enp0s3 udp
  ```

---

## 🔢 Filtering by Port

* 🔒 **Capture HTTPS traffic (port 443)**

  ```bash
  tcpdump -i enp0s3 tcp port 443
  ```

* 🌐 **Capture HTTP-alt/Custom port (e.g. 8080)**

  ```bash
  tcpdump -i enp0s3 tcp port 8080
  ```

* 📡 **Capture DNS over TCP (port 53)**

  ```bash
  tcpdump -i enp0s3 tcp port 53
  ```

---

## 🎯 Advanced Filtering

* 🎯 **Filter by source IP address**

  ```bash
  tcpdump -i enp0s3 src host 192.168.1.7
  ```

* 🎯 **Filter by destination IP address**

  ```bash
  tcpdump -i enp0s3 dst host 192.168.1.100
  ```

* 🌐 **Capture all traffic from a specific subnet**

  ```bash
  tcpdump -i enp0s3 net 192.168.1.0/24
  ```

---

## 💾 Saving and 📖 Reading Packet Captures

### 💾 **Save captured packets to a file**

```bash
tcpdump -i enp0s3 -w capture.pcap
```

### 📖 **Read packets from a capture file**

```bash
tcpdump -r capture.pcap
```

---

## 🧠 Pro Tips

| Option              | Description                              |
| ------------------- | ---------------------------------------- |
| `-i <interface>`    | Specify the network interface            |
| `-n`                | Don't resolve IP addresses to hostnames  |
| `-w <file>`         | Write packets to a file                  |
| `-r <file>`         | Read packets from a file                 |
| `-c <num>`          | Capture only a certain number of packets |
| `-v`, `-vv`, `-vvv` | Increase verbosity of packet output      |

---

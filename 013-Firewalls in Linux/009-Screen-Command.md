# 🧾 **What is `screen`?**

The `screen` command lets you:

* ➕ **Create multiple terminal sessions** within a single terminal window.
* 🔄 **Detach and reattach** to sessions, even after logging out or losing SSH connection.
* 🧩 **Run long-running processes** in the background without interruption.
* 📂 **Open multiple windows (screens)** in a single session, each with its own shell.

---

## 🛠️ **Why Use `screen`?**

Here are some typical use cases:

| Use Case                           | Description                                                                            |
| ---------------------------------- | -------------------------------------------------------------------------------------- |
| 🧪 Running background tasks        | Run processes like `ping`, `top`, or custom scripts without keeping the terminal open. |
| 🔌 Disconnect without killing jobs | If your SSH session drops, your process keeps running.                                 |
| 🔄 Reconnect sessions              | You can reattach to your work exactly where you left off.                              |
| 🪟 Multi-window terminal           | Use one terminal to manage many windows (similar to tabs).                             |

---
## 🚀 Basic Commands

* ▶️ **Start a screen session**

  ```bash
  screen
  ```

* 🏷️ **Start a named session**

  ```bash
  screen -S <session_name>
  ```

  **Example:**

  ```bash
  screen -S mysession
  ```

* ⚙️ **Run a command in the background in a named session**

  ```bash
  screen -S <session_name> -dm <command>
  ```

  **Example:**

  ```bash
  screen -S ping -dm ping 8.8.8.8
  ```

---

## 📋 Managing Sessions

* 📃 **List active sessions**

  ```bash
  screen -ls
  ```

* 🔁 **Reattach the last detached session**

  ```bash
  screen -r
  ```

* 🔧 **Reattach a specific session**

  ```bash
  screen -r <session_name>
  ```

  **Example:**

  ```bash
  screen -r mysession
  ```

---

## 🧠 Advanced Usage

* 🧾 **Run custom scripts in background sessions**

  ```bash
  screen -S <session_name> -dm <script> [options]
  ```

  Example usage: running `nmap-warrior` scripts in background.

---

## 📦 Installing `screen`

* 📌 **On RHEL/CentOS**

  ```bash
  yum install screen
  ```

* 📌 **On Debian/Ubuntu**

  ```bash
  apt install screen
  ```

* 🆘 **Display help**

  ```bash
  screen --help
  ```

---

## 🧪 Usage Example

```bash
# Start a screen session
screen

# Run a command (e.g., ping)
ping 8.8.8.8
# Press Ctrl+A then D to detach

# List active sessions
screen -ls
# Output:
# There is a screen on:
#     1873.pts-0.CENTOS  (Detached)
# 1 Socket in /run/screen/S-root.

# Reattach to the session
screen -r 1873.pts-0.CENTOS
```

Or, run the same ping in background directly:

```bash
screen -S ping -dm ping 8.8.8.8
```

---

## 🛠️ More Session Management

* 🔍 **List active running sessions**

  ```bash
  screen -ls
  ```

* 🧩 **Reattach to the last detached session**

  ```bash
  screen -r
  ```

* 🧷 **Reattach to a specific session**

  ```bash
  screen -r <session_name>
  ```

  **Example:**

  ```bash
  screen -r mysession
  ```

---

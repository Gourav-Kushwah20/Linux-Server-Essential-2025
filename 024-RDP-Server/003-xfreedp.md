# 🖥️ xfreerdp (Fullscreen Mode)

To launch **xfreerdp** in fullscreen mode, use the **+f** option:

```bash
xfreerdp3 /u:clinet-1 /p:123 /v:192.168.1.71 +f
```
* **+f** : Forces xfreerdp to enter fullscreen mode.
Can you see Full screen Display 

## 📺 Set Screen Resolution

To set a specific screen resolution for the remote session, use the **/size** option:

```bash
xfreerdp /u:rahul /p:123 /v:192.168.1.61 +f /size:1920x1080
```

* `/size:1920x1080` : Sets the resolution to Full HD (1920×1080).
* You can adjust the resolution as needed, e.g., **1366×768**, **1280×720**, etc.
## 💾 Redirect Local Drives

To redirect your local drives to the remote session, use the **/drive** option:

```bash
xfreerdp /u:rahul /p:123 /v:192.168.1.61 +f /drive:/opt/share
```

* `/drive:<local_path>` : Redirects a local directory to the remote session.

    ---

## 📋 Redirect Local Clipboard

To enable clipboard sharing between local and remote systems:

```bash
xfreerdp /u:rahul /p:123 /v:192.168.1.61 +f +clipboard
```

* **+clipboard** : Enables clipboard redirection, allowing you to copy and paste between local and remote systems.

---

## 🔊 Redirect Local Audio

To redirect local audio to the remote session, use the **/audio** option:

```bash
xfreerdp /u:rahul /p:123 /v:192.168.1.61 +f +audio
```

* **+audio** : Redirects audio from the remote desktop to the local machine.

---

## 🖱️ Redirect USB Devices

To redirect USB devices (such as a USB mouse or USB storage), use the **/usb** option:

```bash
xfreerdp /u:rahul /p:123 /v:192.168.1.61 +f +usb
```

* **+usb** : Enables USB redirection.

---

## 🔐 Disable TLS Security

If you're connecting to a server that doesn’t support modern encryption, you can use the **/sec** option to change the security mode:

```bash
xfreerdp /u:rahul /p:123 /v:192.168.1.61 +f /sec:rdp
```

* **/sec:rdp** : Forces xfreerdp to use RDP (older, less secure) security mode instead of the default TLS.

---
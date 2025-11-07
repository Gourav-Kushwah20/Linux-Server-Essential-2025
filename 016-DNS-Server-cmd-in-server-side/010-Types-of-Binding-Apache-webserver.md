# Types of Binding in Apache Web Server
- Apache can bind to specific IP address and ports, allowing you to control how and where the web server listens for incoming connections. This document outlines the configuration step and examples of different types of binding.

---
## Check SELinux Status
- check Current SElinux Status:
```bash
sestatus
```

- Modify SELinux configuration:
Edit SELinux configuration file:
```bash
vim /etc/sysconfig/selinux
```
- Set SELinux=disabled

```bash
# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
# See also:
# https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9/html/using_selinux/changing-selinux-states-and-modes_using-selinux#changing-selinux-modes-at-boot-time_changing-selinux-states-and-modes
#
# NOTE: Up to RHEL 8 release included, SELINUX=disabled would also
# fully disable SELinux during boot. If you need a system with SELinux
# fully disabled instead of SELinux running with no policy loaded, you
# need to pass selinux=0 to the kernel command line. You can use grubby
# to persistently set the bootloader to boot with selinux=0:
#
#    grubby --update-kernel ALL --args selinux=0
#
# To revert back to SELinux enabled:
#
#    grubby --update-kernel ALL --remove-args selinux
#
SELINUX=disabled
# SELINUXTYPE= can take one of these three values:
#     targeted - Targeted processes are protected,
#     minimum - Modification of targeted policy. Only selected processes are protected.
#     mls - Multi Level Security protection.
SELINUXTYPE=targeted
```
- Restart System
```bash
reboot
```
- Now you Check apply changes:
```bash
sestatus
SELinux status:                 disabled
```
---
## 2.Types of Apache Binding
Apache can be configured to bind to:
- A specific IP address
- All available IP address
- Multiple ports

---
## 2.1 Binding to All Available IP Address
- By Default,Apache listens on all available IP addresses:
```bash
Listen 80
```
- Apache will Listen on port `80` on all available network interfaces.
- Example output from `netstat`:
```bash
netstat -nltup | grep httpd
```
Output:
```bash
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      907/httpd 
```
- 0.0.0.0 indicates that Apache is listening on all available IP addresses.

---
## 2.2 Binding to Specfic IP address:
- To vind Apache to a specfic Ip Address:

1.Open the Apache Configuration file:
```bash
vim /etc/httpd/conf/httpd.conf
```
2.Modify the `Listen` directive:
```bash
Listen 127.0.0.1:80
```

- This restricts Apache to only listen on 127.0.0.1 (localhost).
```bash
systemctl restart httpd.service
```
- Example output from `netstat`:
```bash
netstat -nltup | grep httpd
```
output:
```bash
tcp        0      0 127.0.0.1:80            0.0.0.0:*               LISTEN      3921/httpd
```

- Open the Browser and Search:
```bash
http://127.0.0.1/
```
---
## 2.3 Binding to a LAN IP Address
To bind Apache to a LAN IP Address(e.g.,`192.168.1.21`)

1.Open the Apache Configuration file:
```bash
vim /etc/httpd/conf/httpd.conf
```
2.Modify the `Listen` directive:
```bash
Listen 192.168.1.21:80
```
3.Restart the Apache Service.
```bash
systemctl restart httpd.service
```

- This allows Apache to listen only on the LAN IP `192.168.1.21`.
- Example output from `netstat`:
```bash
netstat -nltup | grep httpd
```

Output:
```bash
tcp        0      0 192.168.1.21:80         0.0.0.0:*               LISTEN      4244/httpd 
```
- Open the Browser and Search:
```bash
http://192.168.1.21
```
---
## 2.3 Binding to Multiple Ports
Apache can also Listen on multiple ports simultaneously:

1.Open the Apache Configuration file:
```bash
vim /etc/httpd/conf/httpd.conf
```
2.Add multiple `Listen` directives:
```bash
#
#Listen 12.34.56.78:80
#Listen 80
Listen 80
Listen 81
Listen 82
```
- Apache will listen on ports `80`,`81` and `82`.
3.Restart the Apache Service.
```bash
systemctl restart httpd.service
```
- Example output from `netstat`:
```bash
netstat -nltup | grep httpd
```
Output:
```bash
tcp        0      0 0.0.0.0:81              0.0.0.0:*               LISTEN      4642/httpd          
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      4642/httpd          
tcp        0      0 0.0.0.0:82              0.0.0.0:*               LISTEN      4642/httpd
```
```bash
firewall-cmd --permanent --add-port=81/tcp
```
```bash
firewall-cmd --permanent --add-port=82/tcp 
```
```bash
firewall-cmd --reload
```
---
## 3. Restart Apache Service
- After modifying the configuration, restart Apache:
```bash
systemctl restart httpd.service
```
- Check if the Configuration is valid:
```bash
httpd -t
```
---
## 4.Verfy Binding
Use `netstat` to confirm that Apache is listening on the configured ports:
```bash
netstat -nltup | grep httpd
``` 
```bash
tcp        0      0 0.0.0.0:81              0.0.0.0:*               LISTEN      4642/httpd          
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      4642/httpd          
tcp        0      0 0.0.0.0:82              0.0.0.0:*               LISTEN      4642/httpd  
```
- Open the Browser and Search:
```bash
http://192.168.1.21:80
```
```bash
http://192.168.1.21:81
```
```bash
http://192.168.1.21:82
```
![alt text](./img/image-1.png)
![alt text](./img/image.png)
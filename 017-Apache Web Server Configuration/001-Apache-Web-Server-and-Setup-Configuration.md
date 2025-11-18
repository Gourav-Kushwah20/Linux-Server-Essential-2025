# Apache Web Server Setup and Configuration
This Document outlines the installation, Configuration and management of the Apache Web Server on a linux-based system using `yum` and `rpm` package managers. Its includes essential commands, configuration example, and troubleshooting steps.

---

## 1.Check if Apache is installed

- Use the following command to check if Apache (`httpd`) is alerady installed:

```bash
rpm -qa | grep httpd
```
---
## 2. Install Apache Web Server

- Install Apache using `yum:`

```bash
yum install httpd
```

- Install all related Apache packages:

```bash
yum install httpd*
```
---
## 3. Verify Apache Installation

- Check Package Information:
```bash
rpm -qi httpd
```

- List installed files:
```bash
rpm -ql httpd
```

- List Configuration files:
```bash
rpm -qc httpd
```
- List documentation files:
```bash
rpm -qd httpd
```
---

## 4.Apache Configuration
 
### View Apache Configuration

- Check the main configuration files:

```bash
cat /etc/httpd/conf/httpd.conf
```

```bash
tail /etc/httpd/conf/httpd.conf
```
- find the `conf.modules.d` directive:
```bash
grep conf\.modules\.d /etc/httpd/conf/httpd.conf
```
- find the `conf.d` directive:
```bash
grep conf\.d /etc/httpd/conf/httpd.conf
```

---
## 5.Manage Apache Service
### Check Apache Status
- Check if the service is running:
```bash
systemctl status httpd.service
```
- Start the Apache Service:
```bash
systemctl status httpd.service
```
- Enable the Apache to Start on Boot:
```bash
systemctl status httpd.service
```
- Restart the Apache Service:
```bash
systemctl restart httpd.service
```
---
## 6.Network and Port Verfication
### Check Open Ports and Service:
```bash
netstat -nltup
```
- Check if port 80 is open:
```bash
netstat -nltup | grep 80
```
- Check if Apache is listening:
```bash
netstat -nltup | grep httpd 
```
---
## 7.Firewall Configuration
### Firewalld Configuration for Apache Web Server

Steps to configure `firewalld` to allow Apache Web Server traffic. `firewalld` is a firewall management tool that dynamically manages firewall rules on Linux System.

- Check if `firewalld` is running:
```bash
systemctl status firewalld
```

- Start the `firewalld` service:
```bash
systemctl start firewalld
```
- Enable `firewalld` to start automatically at boot:
```bash
systemctl enable firewalld
```
### List Active Services and Ports
- List all service and ports currently open:
```bash
firewall-cmd --list-services
```
```bash
firewall-cmd --list-ports
```
---
## 8.Open Specific Ports for Apache

### Open Port 80 Manually 
- If HTTP is not defined as a service, open port 80 manually:
```bash
firewall-cmd --permanent --add-port=80/tcp
```

### Open Port 443 for HTTPS 
- If HTTPS is not defined as a service, open port 443 manually:
```bash
firewall-cmd --permanent --add-port=443/tcp
```

### Open Additional Port (Optional) 
- For Example, to open port 8080 for a custom Apache configuration:
```bash
firewall-cmd --permanent --add-port=8080/tcp
```
### Reload Firewalld After Adding Ports 
- Reload the Firewall to apply changes:
```bash
firewall-cmd --reload
```
## Check Open Ports
- Verfiy open Ports:
```bash
firewall-cmd --list-ports
```

### Remove Rules(Optional)
### Remove HTTP and HTTPS Service
- If you want to remove HTTP and HTTPS service:
```bash
firewall-cmd --permanent --remove-service=http
```
```bash
firewall-cmd --permanent --remove-service=https
```
### Remove Open Ports
- If you want to remove Specific ports:
```bash
firewall-cmd --permanent --remove-port=80/tcp
```
```bash
firewall-cmd --permanent --remove-port=443/tcp
```

## Reload Firewalld After Removing Rules
- Reload to apply the changes:
```bash
firewall-cmd --reload
```
---
## Configure Firewalld Zones (optional)
- Check Available Zones:
```bash
firewall-cmd --get-zones
```
### Assign  Apache Traffic to a Specfic Zone

- For example, to allow HTTP traffic in the `public` zone:
```bash
firewall-cmd --zone=public --add-service=http --permanent
```
```bash
firewall-cmd --zone=public --add-service=https --permanent
```
- Reload to apply the changes:
```bash
firewall-cmd --reload
```
---
## 9.Apache Process and User Management

### Check Running Process
```bash
ps -aux | grep httpd
```

```bash
lsof | grep httpd
```
### Check Apache User and group
- Find the Apache user:
```bash
cat /etc/passwd | grep apache
```
- Find Apache group:
```bash
cat /etc/group | grep apache
```
--- 
## 10.Configure Website Files
### Create or Edit Web Files
- Navigate to the document root:
```bash
cd /var/www/html
```
- Create or edit an `index.html` file:
```bash
vim /var/www/html/index.html
```
```text
Test123...
```
### Remove Existing File
- Remove the existing file:
```bash
rm -rvf index.html
```
### Set Owership of Web Files
- Set ownership of the website files to Apache user:
```bash
chown -Rv apache:apache /var/www/html/*
```
---
## 11.Load CSS Files
### Download and Extract Template Files
Download template:
```bash
wget https://templatemo.com/tm-zip-files-2020/templatemo_571_hexashop.zip
```

- Unzip the Template:
```bash
unzip templatemo_571_hexashop.zip
```

- Remove the zip File
```bash
rm -rvf templatemo_571_hexashop.zip
```
- Move the extracted folder to a new directory:
```bash
mv -v templatemo_571_hexashop site1
```
- Copy site file to the Apache document root:
```bash
cp -vr templatemo_571_hexashop/* /var/www/html 
```
```bash
chown -Rv apache:apache /var/www/html/*
```

---
### Configure Apache to load CSS Files
- Edit the configuration file:
```bash
vim /etc/httpd/conf/httpd.conf
```

- Add the following line to `httpd.conf` on line 311:
```bash
AddType text/html .shtml
AddType text/css .css
AddOutputFilter INCLUDES .shtml
```
- Restart Apache:
```bash
systemctl restart httpd.service
```
- Set ownership:
```bash
chown -Rv apache:apache /var/www/html/*
```
---
## Test Apache Setup 

### Test Configuration 
- Create a test file:
```bash
Test123...
```
### Check if Apache is running
- Use `curl` to test if Apache is responding:
```bash
curl http://192.168.1.34
```

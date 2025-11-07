# WordPress Installation and Configuration

WordPress is a popular content management system (CMS) used to create and manage websites.

---

## Download and Extract WordPress

### Download the latest version of WordPress from the official website:
```bash
wget https://wordpress.org/latest.zip
````

### Extract the downloaded package:

```bash
unzip latest.zip
```

---

## Move WordPress files to the Apache web directory:

```bash
cp -vr wordpress/ /var/www/html/
```

### Navigate to the web root directory:

```bash
cd /var/www/html/
```

---

## Set the correct ownership to allow Apache to manage WordPress files:

```bash
chown -Rv apache:apache wordpress/
```

### Navigate to the WordPress directory:

```bash
cd wordpress/
```

---

## Set proper permissions for essential WordPress directories:

```bash
chmod -Rv 0755 wp-includes/ wp-admin/js/ wp-content/themes/ wp-content/plugins/
```
---
# Configure Apache for WordPress

### Open the Apache configuration file for editing:
```bash
vim /etc/httpd/conf/httpd.conf
````

### Add the following VirtualHost configuration:

```apache
<VirtualHost 192.168.1.22:443>
    SSLEngine on
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
    DocumentRoot /var/www/html/wordpress/
    DirectoryIndex index.php
</VirtualHost>
```

---

### Restart Apache to apply changes:

```bash
systemctl restart httpd.service
```

### Restart DNS service if applicable:(Optional)

```bash
systemctl restart named.service
```

---

# Apache VirtualHost Configuration

A VirtualHost configuration allows Apache to serve multiple websites from the same server.

---

## Step 1: Configure VirtualHost for SSL

To enable HTTPS support for your website, add the following configuration in the Apache Virtual Host file:

```apache
<VirtualHost 192.168.1.22:443>
    SSLEngine on
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
    ServerName armour.local
    ServerAlias www.armour.local
    DocumentRoot /var/www/html/armour-wp/
    DirectoryIndex index.php
</VirtualHost>
```
- Test domain resolution to ensure WordPress is accessible:

```bash
dig armour.local
```

---
## Configure VirtualHost for HTTP
- If you want your site to be accessible over HTTP (non-secure), use the configuration
```apache
<VirtualHost 192.168.1.22:80>
    ServerName armour.local
    ServerAlias www.armour.local
    DocumentRoot /var/www/html/armour-wp/
    DirectoryIndex index.php
</VirtualHost>
---
layout: default
title: AICWSA Exam Report
---

# WordPress Security Expert Exam Report 

---

- **Rishabh Soni**
- **email@rishabhsoni\.in**
- **17 June 2025**

---
## Table of Contents
---

- [Question](#question)

- [Server Configuration](#server-configuration)  
  - [Credentials](#credentials)  
  - [Server Configuration Steps](#server-configuration-steps)  
    - [Step 1: Initial Server Setup](#step-1-initial-server-setup)  
    - [Step 2: DNS Server Configuration (BIND9)](#step-2-dns-server-configuration-bind9)  
    - [Step 3: Web Stack Installation](#step-3-web-stack-installation)  
      - [3.1 Install Apache2 Web Server](#31-install-apache2-web-server)  
      - [3.2 Install PHP](#32-install-php)  
      - [3.3 Install MySQL Server](#33-install-mysql-server)  
      - [3.4 Install phpMyAdmin](#34-install-phpmyadmin)  
    - [Step 4: Apache Virtual Host Configuration](#step-4-apache-virtual-host-configuration)  
    - [Step 5: File and Database Migration](#step-5-file-and-database-migration)  
    - [Step 6: Configuration and Database URL Update](#step-6-configuration-and-database-url-update)  

- [Security Hardening Steps](#security-hardening-steps)  
  - [File permissions](#file-permissions)  
  - [Implement SSL/TLS](#implement-ssltls)  
  - [Configure a Web Application Firewall (WAF)](#configure-a-web-application-firewall-waf)  
  - [Disable XML-RPC](#disable-xml-rpc)  
  - [Disable REST API Access](#disable-rest-api-access)  
  - [Two-Factor Authentication (2FA)](#two-factor-authentication-2fa)  
  - [Restrict Login Attempts](#restrict-login-attempts)  
  - [Hide the Apache error message](#hide-the-apache-error-message)  
  - [IP Blacklisting](#ip-blacklisting)  
  - [Geolocation Blocking](#geolocation-blocking)  
  - [Set Up Real-Time Activity Monitoring and Alerts](#set-up-real-time-activity-monitoring-and-alerts)  
  - [Installing Recommended Modules](#installing-recommended-modules)  

- [Testing](#testing)  
  - [Successful Migration and Site Functionality Tests](#successful-migration-and-site-functionality-tests)  
    - [Initial Site Load and SSL/TLS](#initial-site-load-and-ssltls)  
    - [Permalinks and Internal Navigation](#permalinks-and-internal-navigation)  
    - [Plugins and Theme Functionality](#plugins-and-theme-functionality)  
    - [File Permissions](#file-permissions)  
  - [Security Hardening Verification Tests](#security-hardening-verification-tests)  
    - [Web Application Firewall (WAF)](#web-application-firewall-waf)  
    - [XML-RPC Endpoint Disabled](#xml-rpc-endpoint-disabled)  
    - [REST API Restriction](#rest-api-restriction)  
    - [Two-Factor Authentication (2FA)](#two-factor-authentication-2fa-1)  
    - [Brute-Force Login Attempt Restriction](#brute-force-login-attempt-restriction)  
    - [IP Blacklisting and Geoblocking](#ip-blacklisting-and-geoblocking)  
    - [Monitoring and Alert Systems](#monitoring-and-alert-systems)  


---
## Question
---
### Task: Manually Migrate a WordPress Site to a New Server

You are tasked with migrating a WordPress site from an old Debian 10 server running Apache to a new Debian 12 server, also running Apache. This must be done manually without the use of migration plugins. Below are the instructions and important details for completing the task.

Task Details:
- **First Zip:** Contains all WordPress site files (themes, plugins, uploads, etc). Download it from [here](https://drive.google.com/file/d/1vw-ekfJ-eAjerilcjXwsQxlhSl8QnHTZ/view?usp=sharing) ([https://drive.google.com/file/d/1vw-ekfJ-eAjerilcjXwsQxlhSl8QnHTZ/view?usp=sharing](https://drive.google.com/file/d/1vw-ekfJ-eAjerilcjXwsQxlhSl8QnHTZ/view?usp=sharing)).
- **Second Zip:** Contains the exported WordPress database. Download it from [here](https://drive.google.com/file/d/1MsWTCgtkVYzDYf0TEmTavb_63ZCD6J9N/view?usp=sharing) ([https://drive.google.com/file/d/1MsWTCgtkVYzDYf0TEmTavb_63ZCD6J9N/view?usp=sharing](https://drive.google.com/file/d/1MsWTCgtkVYzDYf0TEmTavb_63ZCD6J9N/view?usp=sharing)).

Instructions:
- Extract the WordPress site files from the first zip and transfer them to the new server.
- Import the database from the second zip file to the new server’s database.
- Update the `wp-config.php` file to reflect the new database and server settings.
- Ensure all file and directory permissions are properly set.
- Test the site on the new server to confirm that everything works correctly (e.g., permalinks, plugins, themes).

Security Hardening for **lms.armour.local**:
- Implement SSL/TLS by obtaining and configuring an SSL certificate.
- Set appropriate file and directory permissions to secure the site.
- Configure a Web Application Firewall (WAF) to enhance security.
- Disable unnecessary features like XML-RPC and restrict REST API access for unauthorized users.
- Enable Two-Factor Authentication (2FA) for admin accounts.
- Set up real-time monitoring and alert systems to notify administrators of suspicious activities.
- Use IP blacklisting and geoblocking to prevent malicious traffic from specific regions or IP addresses.
- Restrict login attempts to prevent brute-force attacks.

Deliverables:
- A detailed configuration document (PDF) outlining the steps taken for migration, including the updated `wp-config.php` file (with sensitive information redacted).
- Screenshots or logs confirming the successful migration and testing of the site.

Assessment Criteria:
- Accurate extraction and transfer of files and database.
- Correct configuration of the WordPress site on the new server.
- Site functionality post-migration without errors.


---
## Server Configuration
---
**OS:** Debian 12
**IPv4:** 192.168.1.128/24
**Gateway:** 192.168.1.1
**Hostname:** debian.armour.local
**Domain:** lms.armour.local

### Credentials
---
**Server Users:**

| Users | Password   | Type  |
| ----- | ---------- | ----- |
| root  | Rish@bh123 | admin |
| rsoni | Rish@bh123 | admin |

**Service Credentials:**

| Service      | Username        | Password   | Database/Site    |
| ------------ | --------------- | ---------- | ---------------- |
| MySQL (root) | root            | Rish@bh123 | N/A              |
| MySQL (WP)   | lms_armour_user | Rish@bh123 | armour_lms       |
| WordPress    | admin           | Rish@bh123 | lms.armour.local |
| WordPress    | armour          | Rish@bh123 | lms.armour.local |


---
## Server Configuration Steps
---
### Step 1: Initial Server Setup
---
Set the system hostname.

```bash
hostnamectl set-hostname debian.armour.local
```

Configure a static IP address using your preferred network management tool.

Install essential tools for administration and networking.

```bash
apt install openssh-server vim bash* unzip network-manager curl net-tools -y
```

### Step 2: DNS Server Configuration (BIND9)
---
Install the BIND9 DNS server and related utilities.

```bash
apt install bind9 bind9-dnsutils -y
```

Edit the main options file to configure listeners and access controls.

```bash
vim /etc/bind/named.conf.options
```

```ini
options {
        listen-on port 53 { 127.0.0.1; 192.168.1.128; };
        listen-on-v6 { none; };
        allow-query { 127.0.0.1; 192.168.1.0/24; };
        recursion yes;
        forwarders { 8.8.8.8; 8.8.4.4; };
};
```

Edit the local configuration file to define the new zones.

```bash
vim /etc/bind/named.conf.local
```

```
zone "armour.local" {
    type master;
    file "/etc/bind/zones/db.armour.local";
};

zone "1.168.192.in-addr.arpa" {
    type master;
    file "/etc/bind/zones/db.192.168.1";
};
```

Create the directory to store the zone files.

```bash
mkdir -p /etc/bind/zones
```

Create the forward zone file for `armour.local`.

```bash
vim /etc/bind/zones/db.armour.local
```

```
$TTL    604800
@       IN      SOA     debian.armour.local. root.armour.local. (
                              3         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      debian.armour.local.
debian  IN      A       192.168.1.128
lms     IN      A       192.168.1.128
www.lms IN      CNAME   lms.armour.local.
```

Create the reverse zone file for the `192.168.1.0/24` network.

```bash
vim /etc/bind/zones/db.192.168.1
```

```
$TTL    604800
@       IN      SOA     debian.armour.local. root.armour.local. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
@       IN      NS      debian.armour.local.
128     IN      PTR     debian.armour.local.
```

Check the syntax of the configuration files.

```bash
named-checkconf
```

```bash
named-checkzone armour.local /etc/bind/zones/db.armour.local
```

```bash
named-checkzone 1.168.192.in-addr.arpa /etc/bind/zones/db.192.168.1
```

Restart the BIND9 service to apply all changes.

```bash
systemctl restart bind9
```

### Step 3: Web Stack Installation
---
#### 3.1 Install Apache2 Web Server

Install the Apache2 package.

```bash
apt install apache2 -y
```

Disable directory listing in the main Apache configuration for better security.

```bash
sed -i "s/Options Indexes FollowSymLinks/Options FollowSymLinks/" /etc/apache2/apache2.conf
```

Enable the Apache SSL module.

```bash
a2enmod ssl
```

Enable the Apache Rewrite module.

```bash
 a2enmod rewrite
```

Create a dedicated directory for SSL certificates.

```bash
mkdir /etc/apache2/ssl
```

Generate a self-signed certificate for the domain.

```bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/apache2/ssl/armour.local.key -out /etc/apache2/ssl/armour.local.crt
```

Restart Apache to load the new SSL module.

```bash
systemctl restart apache2
```

#### 3.2 Install PHP

Install PHP 8.2 and all required extensions for WordPress and related tools.

```bash
apt install php php8.2 php8.2-common php8.2-mbstring php8.2-xmlrpc php8.2-soap php8.2-gd php8.2-xml php8.2-intl php8.2-mysql php8.2-cli php8.2-ldap php8.2-zip php8.2-curl php-xml composer -y
```

Edit the `php.ini` file for Apache to set optimal values for WordPress.

```bash
vim /etc/php/8.2/apache2/php.ini
```

```ini
memory_limit = 512M
max_execution_time = 120
max_input_vars = 3000
upload_max_filesize = 64M
post_max_size = 64M
allow_url_fopen = On
```

Restart Apache to apply the new PHP configuration.

```bash
systemctl restart apache2
```

#### 3.3 Install MySQL Server

Download the MySQL repository configuration package.

```bash
wget https://dev.mysql.com/get/mysql-apt-config_0.8.34-1_all.deb
```

Install the package to add the repository.

```bash
apt install ./mysql-apt-config_0.8.34-1_all.deb
```
> In the interactive prompt that appears, select **MySQL Server 8.4 LTS**.

Update the APT package list to include the new repository.

```bash
apt update
```

Install the MySQL server package and `pwgen` utility.

```bash
apt install mysql-community-server pwgen -y
```

Run the security script to set the root password and harden the installation.

```bash
mysql_secure_installation
```
> Set the root password to: **Rish@bh123** and answer yes (**y**) to all subsequent security questions.

Enable the MySQL service to start on boot.

```bash
systemctl enable mysql.service
```

Start the MySQL service.

```bash
systemctl start mysql.service
```

#### 3.4 Install phpMyAdmin

Download the phpMyAdmin archive.

```bash
wget https://files.phpmyadmin.net/phpMyAdmin/5.2.0/phpMyAdmin-5.2.0-all-languages.zip
```

Unzip the archive.

```bash
unzip phpMyAdmin-5.2.0-all-languages.zip
```

Move the extracted folder to the web root with a simpler name.

```bash
mv -v phpMyAdmin-5.2.0-all-languages /var/www/html/phpmyadmin
```

Create the configuration file from the provided sample.

```bash
cp -v /var/www/html/phpmyadmin/config.sample.inc.php /var/www/html/phpmyadmin/config.inc.php
```

Generate a random 32-character string for the blowfish secret.

```bash
pwgen 32 1
```
> Example Output: `pie6KuSoyohee4joh4xee3aevohCio3a`

Edit the configuration file to add the secret.

```bash
vim /var/www/html/phpmyadmin/config.inc.php
```
> Update the line: `$cfg['blowfish_secret'] = 'pie6KuSoyohee4joh4xee3aevohCio3a';`

Set the correct ownership for the phpMyAdmin directory.

```bash
chown -Rv www-data:www-data /var/www/html/phpmyadmin
```

### Step 4: Apache Virtual Host Configuration
---
Create the VirtualHost configuration file for the WordPress site.

```bash
vim /etc/apache2/sites-available/lms.armour.local.conf
```

```apache
# Redirect HTTP to HTTPS for lms.armour.local
<VirtualHost *:80>
    ServerName lms.armour.local
    ServerAlias www.lms.armour.local
    Redirect permanent / https://lms.armour.local/
    ErrorLog ${APACHE_LOG_DIR}/lms.armour.local-http-error.log
    CustomLog ${APACHE_LOG_DIR}/lms.armour.local-http-access.log combined
</VirtualHost>

# Main configuration for lms.armour.local over HTTPS
<VirtualHost *:443>
    ServerAdmin root@armour.local
    ServerName lms.armour.local
    ServerAlias www.lms.armour.local
    DocumentRoot /var/www/html/armour.local
    <Directory /var/www/html/armour.local>
        Options FollowSymLinks
        AllowOverride All
        Require all granted
        DirectoryIndex index.php
    </Directory>
    # SSL Configuration
    SSLEngine on
    SSLCertificateFile /etc/apache2/ssl/armour.local.crt
    SSLCertificateKeyFile /etc/apache2/ssl/armour.local.key
    # Improved Logging
    ErrorLog ${APACHE_LOG_DIR}/lms.armour.local-ssl-error.log
    CustomLog ${APACHE_LOG_DIR}/lms.armour.local-ssl-access.log combined
</VirtualHost>
```

Create a default block-all VirtualHost to prevent direct IP access.

```bash
vim /etc/apache2/sites-available/000-default-block.conf
```

```apache
<VirtualHost *:80>
    ServerName default-server
    <Location />
        Require all denied
    </Location>
    ErrorLog ${APACHE_LOG_DIR}/default-blocked-error.log
    CustomLog ${APACHE_LOG_DIR}/default-blocked-access.log combined
</VirtualHost>

<VirtualHost *:443>
    ServerName default-server
    SSLEngine on
    SSLCertificateFile /etc/apache2/ssl/armour.local.crt
    SSLCertificateKeyFile /etc/apache2/ssl/armour.local.key
    <Location />
        Require all denied
    </Location>
    ErrorLog ${APACHE_LOG_DIR}/default-ssl-blocked-error.log
    CustomLog ${APACHE_LOG_DIR}/default-ssl-blocked-access.log combined
</VirtualHost>
```

Enable the new site for the domain.

```bash
a2ensite lms.armour.local.conf
```

Enable the default catch-all block.

```bash
a2ensite 000-default-block.conf
```

Disable the default Apache site.

```bash
a2dissite 000-default.conf
```

Disable the default SSL site.

```bash
a2dissite default-ssl.conf
```

Reload Apache to apply the new site configurations.

```bash
systemctl reload apache2
```

### Step 5: File and Database Migration
---
Extract the provided backup files archive.

```bash
unzip backupfiles.zip
```

Move the WordPress site files into the Apache document root.

```bash
mv armour.local/ /var/www/html/
```

Recursively set the correct ownership for all site files.

```bash
chown -Rv www-data:www-data /var/www/html/armour.local
```

Log in to phpMyAdmin (`http://192.168.1.128/phpmyadmin`) and perform the following:
1.  Create a new database named `armour_lms`.
2.  Select the `armour_lms` database.
3.  Click the "Import" tab and upload the `backupdb.zip` file.

### Step 6: Configuration and Database URL Update
---
Log into MySQL to create a dedicated user for the WordPress site.

```bash
mysql -u root -p
```

```sql
CREATE USER 'lms_armour_user'@'localhost' IDENTIFIED BY '**********';
```

```sql
GRANT ALL PRIVILEGES ON armour_lms.* TO 'lms_armour_user'@'localhost';
```

```sql
FLUSH PRIVILEGES;
```

```sql
EXIT;
```


Change the Wordpress admin and users password
 
 ```sql
UPDATE wp_users SET user_pass = MD5('Rish@bh123') WHERE user_login = 'admin';
```

```sql
UPDATE wp_users SET user_pass = MD5('Rish@bh123') WHERE user_login = 'armour';
```

Edit the WordPress configuration file.

```bash
vim /var/www/html/armour.local/wp-config.php
```

Update the file with the new dedicated database credentials.

```php
/** The name of the database for WordPress */
define( 'DB_NAME', 'armour_lms' );

/** Database username */
define( 'DB_USER', 'lms_armour_user' );

/** Database password */
define( 'DB_PASSWORD', '**********' );

// ... other settings

/** Table prefix */
$table_prefix = 'wp_';
```

Log into MySQL again to update the site URLs in the database.

```bash
mysql -u root -p
```

```sql
USE armour_lms;
```

```sql
UPDATE wp_options SET option_value = 'https://lms.armour.local' WHERE option_name = 'siteurl';
```

```sql
UPDATE wp_options SET option_value = 'https://lms.armour.local' WHERE option_name = 'home';
```

```sql
UPDATE wp_posts SET post_content = REPLACE(post_content, 'https://www.armour.local', 'https://lms.armour.local');
```

```sql
UPDATE wp_posts SET guid = REPLACE(guid, 'https://www.armour.local', 'https://lms.armour.local');
```

```sql
UPDATE wp_options SET option_value = REPLACE(option_value, 'https://www.armour.local', 'https://lms.armour.local');
```


After migration, outdated URLs (`http://www.armour.local`) caused broken links due to Elementor's serialized data. To fix:

1. Logged into WordPress admin.
2. Went to **Elementor → Tools → Replace URL**.
3. Entered old URL (`http://www.armour.local`) and new URL (`https://lms.armour.local`).
4. Clicked **Replace URL** to update all references, including serialized data.
5. Under **Elementor → Tools → General**, clicked **Regenerate Files & Data**.    
6. Went to **Settings → Permalinks** and clicked **Save Changes** to flush rewrite rules.


Restart the primary services to apply all configuration changes.

```bash
systemctl restart mysql.service apache2.service
```


---
## Security Hardening Steps  
---
### File permissions

Set ownership for all files and directories:  

```bash
chown -R www-data:www-data /var/www/html/armour.local/
```

Set permissions for all directories to 755:  
```bash
find /var/www/html/armour.local/ -type d -exec sudo chmod 755 {} \;
```

Set permissions for all files to 644:  
```bash
find /var/www/html/armour.local/ -type f -exec sudo chmod 644 {} \;
```

Set secure permissions for the `wp-config.php` file:  
```bash
chmod 640 /var/www/html/armour.local/wp-config.php
```

Set permissions for the `.htaccess` file:  
```bash
chmod 644 /var/www/html/armour.local/.htaccess
```

---
### Implement SSL/TLS

Implemented in **Step 3.1** and **Step 4** by generating a self-signed certificate and configuring Apache VirtualHost for HTTPS on port 443.

**Permalink Regeneration:**  
WordPress Admin → **Settings → Permalinks** → Click **Save Changes** (no edits). This flushes rewrite rules and rebuilds `.htaccess`.

**Admin Password Reset:**  
Updated strong passwords for `admin` and `armour` users via **Users → Profile** after migration.


---
### Configure a Web Application Firewall (WAF)

**Plugin: All-In-One Security (AIOS)**

**Block malicious requests**  

→ _WP Security → Firewall → .htaccess Rules_
- Enable basic firewall settings    
- Enable basic firewall protection


**Filter suspicious character strings in HTTP requests**  

→ _WP Security → Firewall → PHP Rules → String Filtering_
- Enable advanced character string filter


**Block malformed or malicious query strings**  

→ _WP Security → Firewall → PHP Rules → URL Security_
- Deny bad query strings    


**Prevent spam or proxy-based comment exploits**  

→ _WP Security → Firewall → PHP Rules → Comment Protection_
- Forbid proxy comment posting


**Block fake bots and suspicious POST requests**  

→ _WP Security → Firewall → Internet Bots_
- Block fake Google bots
- Ban POST requests with a blank user-agent and refferer
 
---
### Disable XML-RPC

 **Plugin: All-In-One Security (AIOS)**
 
→ _WP Security → Firewall → PHP Rules → String Enhancements_
- Completely block access to XML-RPC
- Disable pingback functionality from XML-RPC

---
### Disable REST API Access

 **Plugin: All-In-One Security (AIOS)**
 
→ _WP Security → Firewall → PHP Rules → WP REST API_
- Disallow    

---
### Two-Factor Authentication (2FA)

 **Plugin: All-In-One Security (AIOS)**
 
→ _WP Security → Two Factor Auth_
- Activate two-factor auth
- Enable  

Use a private key and register it in an authenticator app (e.g., Google Authenticator or Authy).

---
### Restrict Login Attempts

 **Plugin: Limit Login Attempts Reloaded**

 **Settings:**  
-  allowed retries = 5  
-  minutes lockout = 20 min  
 - lockouts increase lockout time to = 24 hours  
 - hours until retries are reset = 24 hours  

---
### Hide the Apache error message

Edit the Apache security configuration file to hide server version details.  

```bash
vim /etc/apache2/conf-enabled/security.conf
```

Change the following lines:  

```apache
ServerTokens Prod
ServerSignature Off
```

Restart Apache to apply the change:  

```bash
systemctl restart apache2
```

---
---

### IP Blacklisting

 **Plugin: All-In-One Security (AIOS)**
 
→ _WP Security → Firewall → Blocks & Allow Lists → IP or User Agent Blacklisting_
- Add blocked IP: `192.168.1.201`

---
### Geolocation Blocking

**Plugin: iQ Block Country**

→ _WP Admin → Settings → iQ Block Country → Front-end Tab_
- Block visitors from selected countries (e.g., RU, CN)
- Use GeoLite2 database (uploaded to `/wp-content/uploads/GeoLite2-Country.mmdb`)    
- Requires a free license key from MaxMind

---
### Set Up Real-Time Activity Monitoring and Alerts

**Plugin: WP Activity Log**

→ _WP Activity Log → Enable/Disable Events_
- Enable logging for Medium, High, and Critical severity events

→ _WP Activity Log → Notifications → Email Notifications_
- Add admin email and enable daily activity summary

---
### Installing Recommended Modules:

```bash
apt install php-imagick -y
```

```bash
systemctl restart apache2
```


---
## Testing  
---
### Successful Migration and Site Functionality Tests
---
#### Initial Site Load and SSL/TLS  

![[SS-01-Homepage-Load.png.png]]

----
#### Permalinks and Internal Navigation  

![[SS-02-Perma-Links-1.png]]

![[SS-03-Perma-Links-2.png]]

---
#### Plugins and Theme Functionality  

![[Pasted image 20250617004718.png]]

![[Pasted image 20250617004746.png]]

---
#### File Permissions  

```bash
ls -ld /var/www/html/armour.local/ /var/www/html/armour.local/wp-includes/ /var/www/html/armour.local/.htaccess /var/www/html/armour.local/wp-admin/index.php /var/www/html/armour.local/wp-admin/js/ /var/www/html/armour.local/wp-content/themes/ /var/www/html/armour.local/wp-content/plugins/ /var/www/html/armour.local/wp-admin/ /var/www/html/armour.local/wp-content/ /var/www/html/armour.local/wp-config.php
```

![[Pasted image 20250617004347.png]]

![[Pasted image 20250617003007.png]]

---
### Security Hardening Verification Tests
---
#### Web Application Firewall (WAF)

```
https://lms.armour.local/?test=<script>alert('waf-test')</script>
```

```
https://lms.armour.local/?id=1';DROP TABLE users;--
```

```
https://lms.armour.local/?url=javascript:alert('XSS')
```

![[Pasted image 20250617005254.png]]

---
#### XML-RPC Endpoint Disabled

```
https://lms.armour.local/xmlrpc.php
```

![[SS-08-XML-RPC.png.png]]

---
#### REST API Restriction

Test these endpoints to confirm restricted or authorized access:

```
https://lms.armour.local/wp-json/wp/v2/users
```

```
https://lms.armour.local/wp-json/wp/v2/posts
```

```
https://lms.armour.local/wp-json/wp/v2/comments
```

```
https://lms.armour.local/wp-json/wp/v2/media
```

```
https://lms.armour.local/wp-json/wp/v2/categories
```


**Unauthorized User:**  

![[Pasted image 20250617005328.png]]

**Logged in user:**  

![[Pasted image 20250617005350.png]]

---
#### Two-Factor Authentication (2FA)

![[Pasted image 20250617005531.png]]

---
#### Brute-Force Login Attempt Restriction

![[Pasted image 20250617005620.png]]

![[Pasted image 20250617005646.png]]

---
#### IP Blacklisting and Geoblocking

**Blocked IP: 192.168.1.201**  

![[Pasted image 20250617010116.png]]

![[Pasted image 20250617010425.png]]

**Blocked Geolocations:**  

![[Pasted image 20250617010533.png]]

---
#### Monitoring and Alert Systems

![[Pasted image 20250617005840.png]]

Properly configured to send alerts on email.

---
---

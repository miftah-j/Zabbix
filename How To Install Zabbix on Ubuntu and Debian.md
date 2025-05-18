
# How To Install Zabbix on Ubuntu and Debian


## Introduction

Zabbix is a widely used open-source monitoring solution for IT infrastructure and applications, trusted by businesses of all sizes worldwide.

This guide will walk you through the installation process of Zabbix on  **Ubuntu 22.04 & 24.04**  and  **Debian 12**, including database setup, Zabbix server configuration, and web frontend setup.

## Prerequisites

**MariaDB:**  To run Zabbix, you must have MariaDB installed. If it's not already on your server, you can follow  [our guide to install it.](https://community.time4vps.com/discussion/8020/how-to-install-mariadb-on-ubuntu-debian/p1 "our guide to install it.")

**Important:**  The Apache web server will be automatically installed as part of the Zabbix setup process.

## Installation Guide

### 1. Add the Zabbix Repository

Download and install the Zabbix repository package for your OS:

**Ubuntu 24.04**

`wget https://repo.zabbix.com/zabbix/7.2/release/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.2+ubuntu24.04_all.deb`  
`dpkg -i zabbix-release_latest_7.2+ubuntu24.04_all.deb`

**Ubuntu 22.04**

`wget https://repo.zabbix.com/zabbix/7.2/release/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.2+ubuntu22.04_all.deb`  
`dpkg -i zabbix-release_latest_7.2+ubuntu22.04_all.deb`

**Debian 12**

`wget https://repo.zabbix.com/zabbix/7.2/release/debian/pool/main/z/zabbix-release/zabbix-release_latest_7.2+debian12_all.deb`  
`dpkg -i zabbix-release_latest_7.2+debian12_all.deb`

### 2. Update Packages and Install Zabbix

Update the package list and install Zabbix server, frontend, agent, and dependencies:

`apt update`  
`apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent`

### 3. Configure MySQL Database for Zabbix

Access MySQL:

`mysql -u root -p`

Create the database and user:

`create database zabbix character set utf8mb4 collate utf8mb4_bin;`  
`create user zabbix@localhost identified by 'your_password';`  
`grant all privileges on zabbix.* to zabbix@localhost;`  
`set global log_bin_trust_function_creators = 1;`  
`quit;`

Import the initial schema:

`zcat /usr/share/zabbix/sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix`

You will be prompted to enter the password you set for the Zabbix database user.

Next, disable log_bin_trust_function_creators:

`mysql -u root -p`  
`set global log_bin_trust_function_creators = 0;`  
`quit;`

### 4. Configure Zabbix Server

Edit the Zabbix server configuration file:

`nano /etc/zabbix/zabbix_server.conf`

Enter the database password that you specified for your zabbix database user:

> DBPassword=your_password

Save and exit (Ctrl+X, Y, Enter).

### 5. Start and Enable Zabbix Services

Restart and enable the Zabbix server, agent, and Apache services:

`systemctl restart zabbix-server zabbix-agent apache2`  
`systemctl enable zabbix-server zabbix-agent apache2`

### 6. Secure with Let's Encrypt SSL Certificate

Install Certbot and generate an SSL certificate for your domain:

`apt install certbot python3-certbot-apache -y`  
`certbot --apache -d your.vps.hostname`

### 7. Restart Apache and Zabbix

Restart Apache and the Zabbix server to apply changes:

`systemctl restart apache2`  
`systemctl restart zabbix-server`

Zabbix is now installed and ready for configuration via the web interface!

## Zabbix frontend setup

### 1. Access the Zabbix Web Interface

Open your browser and access the Zabbix web interface by entering your server's hostname followed by** /zabbix**:

`https://your-server-hostname/zabbix`

You will see the page where you can set up your Zabbix.

### 2. Select Language

The first page will prompt you to select your preferred language for the installation and show the version of Zabbix being installed. Choose your language and click 'Next step':

![](https://community.time4vps.com/uploads/editor/dv/nkrvzjxum12o.png)

### 3. Check of the pre-requisites

Zabbix will check if the required PHP versions and configurations are present on your system. Make sure all the prerequisites are met before continuing.

![](https://community.time4vps.com/uploads/editor/tc/qhvwtg8ux0pm.png)

### Important!

if you see an error regarding the Locale ("Locale for language "en_US" is not found on the web server."), then please [follow this guide to solve this problem](https://community.time4vps.com/discussion/8042/debian-ubuntu-fixing-locale-issues-in-linux-falling-back-to-the-standard-locale-c#latest "follow this guide to solve this problem").

![](https://community.time4vps.com/uploads/editor/ze/lh887fzn90pq.png)

After that, restart Apache and Zabbix and try to complete the Zabbix interface configuration again:

`systemctl restart apache2`  
`systemctl restart zabbix-server`

### 4. Configure Db connection

Next, enter the database details, including the database name, username, and password that you configured earlier:

![](https://community.time4vps.com/uploads/editor/u0/l6uanz5vzo9o.png)

### 5. Other settings

Next, configure the server name, set the time zone, and choose the default theme for the Zabbix frontend:

![](https://community.time4vps.com/uploads/editor/72/1nci8mno25hd.png)

### 6. Summary

The system will display a summary of the configurations and checks. Click "Next step" to complete the installation process:

![](https://community.time4vps.com/uploads/editor/fw/dx1t27xz2m31.png)

![](https://community.time4vps.com/uploads/editor/78/wyoj9vicfvkr.png)

### 7. Login to Zabbix

Now you can access your Zabbix dashboard. Use the default Admin credentials to log in:

**Username:**  Admin  
**Password:**  zabbix

After logging in, you will be directed to the Zabbix dashboard, where you can begin configuring and monitoring your systems.

However, we highly recommend changing the default Admin password for security purposes.

To do this, click on "**User settings**" in the lower right corner:

![](https://community.time4vps.com/uploads/editor/1h/216y9d2l91jj.png)

Then, click "**Profile**":

![](https://community.time4vps.com/uploads/editor/vj/sv0v4tvcyl3o.png)

Next, click on "**Change password**":

![](https://community.time4vps.com/uploads/editor/cs/k2uww9iztwur.png)

Enter your current password, then set a new one, and click "Update" to save the changes. For enhanced security, use a 12-16 character password that includes uppercase and lowercase letters, numbers, and special characters.

![](https://community.time4vps.com/uploads/editor/q4/gko9srr54ku2.png)

Once you've updated your password, you will need to log in again using the new credentials.

## Conclusion

By installing Zabbix, you establish a robust monitoring system for your infrastructure. After completing the setup, configuring the database, and securing the installation, you'll be able to efficiently monitor performance and quickly identify potential issues.

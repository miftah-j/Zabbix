

# How to Install Zabbix Monitoring Tool on Ubuntu 22.04


Zabbix is an open-source monitoring tool designed for enterprise-level monitoring of IT environments, including servers, networks, applications, and services. It helps organizations proactively monitor and manage their IT infrastructure by collecting metrics, analyzing data, and generating alerts or notifications.



## Update & upgrade the system.

First Update your system to ensure all packages are up to date

    sudo apt update 
    sudo apt upgrade -y



## Install Apache2

    sudo apt install apache2 -y


**Enable Apache to Start on boot**

    sudo systemctl enable apache2
    sudo systemctl start apache2

**Open Firewall for HTTP & HTTPS:**

    sudo ufw allow in "Apache Full"


## Install MySQL/MariaDB Server

**Install MariaDB Server**

    sudo apt install mariadb-server -y


Start and enable MariaDB to runboot.oot,

    sudo systemctl start mariadb
    sudo systemctl enable mariadb


**Secure MySQL Installation**

    sudo mysql_secure_installation


## Create Initial Database & User for Zabbix

    sudo mysql -u root -p

Inside the MySQL shell, run:

    CREATE DATABASE zabbix CHARACTER SET utf8 COLLATE utf8_bin;
    CREATE USER 'zabbix'@'localhost' IDENTIFIED BY 'zabbix';
    GRANT ALL PRIVILEGES ON zabbix.* TO 'zabbix'@'localhost';
    SET GLOBAL log_bin_trust_function_creators = 1;
    FLUSH PRIVILEGES;
    EXIT;

Note: Replace 'zabbix' with a secure password of your choice.
 

## Install Zabbix Server & Agent

**Add Zabbix Repository:**

Run the following commands to install the Zabbix repository, Download Zabbix repository for Ubuntu 22.04 (Zabbix 7.2)


    wget https://repo.zabbix.com/zabbix/7.2/release/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.2+ubuntu22.04_all.deb
    
    sudo dpkg -i zabbix-release_latest_7.2+ubuntu22.04_all.deb
    
    sudo apt update


**Install Zabbix Server, Frontend, and Agent**

    sudo apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent -y

## Configure Zabbix Database

**Import initial Schema:**

    sudo zcat /usr/share/zabbix/sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix

Enter the password for the Zabbix MySQL user when prompted.


Disable log_bin_trust_function_creators option after importing databse schema.


Enter the MariaDB root password when prompted. Create the Zabbix database and user

    sudo mysql -uroot -p

set global log_bin_trust_function_creators = 0;
quit;



Configure Zabbix Server Database
________________________________________

Edit the Zabbix server configuration file to set the database password, Open the Zabbix server configuration file,

    sudo nano /etc/zabbix/zabbix_server.conf


Find the DBPassword line and update it with your database password,

    DBHost=localhost
    DBName=zabbix
    DBUser=zabbix
    DBPassword=zabbix

Note: Replace Your_Password_Here with your actual Strong Password


Configure PHP for Zabbix Frontend
________________________________________

Edit the Apache configuration file for Zabbix frontend to enable PHP, Open the Zabbix Apache configuration file,



    sudo nano /etc/zabbix/apache.conf


Find and adjust the php_valuesetting as needed:

    php_value max_execution_time 300
    php_value memory_limit 128M
    php_value post_max_size 16M
    php_value upload_max_filesize 2M
    php_value max_input_time 300
    php_value max_input_vars 10000
    php_value date.timezone Asia/Dhaka


Save & Exit.

Start Zabbix Server & Apache2
 

    sudo systemctl restart apache2
    
    sudo systemctl enable zabbix-server zabbix-agent
    
    sudo systemctl start zabbix-server zabbix-agent


Verify the Zabbix Server Status;

    sudo systemctl status zabbix-server


Configure Zabbix Server Frontend

Access Zabbix Web Interface: Open your Web browser & navigate to:

https://192.168.221.141/Zabbix

http://your-server-ip-or-domain:8080

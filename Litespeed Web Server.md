---
layout: single
type: docs
permalink: /docs/installation/providers/enterprise/litespeed-ubuntu/
redirect_from:
  - /theme-setup/
last_modified_at: 2023-07-14
toc: true
title: Installing Faveo Helpdesk on Ubuntu With Litespeed Web Server
---


<img alt="Ubuntu" src="/Images/litespeed-webserver-logo.png" width="120" height="120" />

Faveo can run on [Ubuntu 20.04 (Focal Fosa), Ubuntu 22.04 (Jammy Jellyfish)](http://releases.ubuntu.com/22.04/).


# Installation steps
Faveo depends on the following:

- <strong>Apache</strong> (with mod_rewrite enabled)
- <strong>PHP 8.1+</strong> with the following extensions: curl, dom, gd, json, mbstring, openssl, pdo_mysql, tokenizer, zip
- <strong>MySQL 8.0+ or MariaDB 10.6+</strong>
- <strong>SSL</strong> ,Trusted CA Signed or Slef-Signed SSL


### 1. LOMP Installation

The LOMP stack is an acronym for Linux, OpenLiteSpeed, MariaDB, and PHP. OpenLiteSpeed is the open-source option for LiteSpeed web servers. 

In this tutorial, you will set up a LOMP server running on Ubuntu 22.04. At the time of writing, the current versions are PHP 8.1, MariaDB 10.7, and OpenLiteSpeed 1.7.

Run the following commands as sudoers or Login as root user by typing the command below

```
sudo su
```
### 1.a. Update your package list

```
apt update && apt upgrade -y
```

### 1.b.  Installing OpenLiteSpeed With LSPHP 8.1

OpenLiteSpeed hosts its code on its own repository. Add this repository to the apt package manager’s sources list with the following command:

```
sudo wget -O - https://repo.litespeed.sh | sudo bash
```



Update the list of repositories to ensure that the newly added repository is scanned by the apt package manager:

```
sudo apt update
```
Next, install the openlitespeed package:

```
sudo apt install openlitespeed lsphp81 lsphp81-curl lsphp81-imap lsphp81-mysql lsphp81-ldap lsphp81-redis lsphp81-ioncube 
```
If prompted, enter your password, then confirm the installation with Y.

If prompted, enter your password, then confirm the installation with Y.

This command installs the Openlitespeed server package and LSPHP 8.1. [LiteSpeed PHP (LSPHP)](https://docs.litespeedtech.com/lsws/extapp/php/configuration/options/) is a PHP interpreter integrated with the [LiteSpeed Server Application Programming Interface (LSAPI)](https://www.litespeedtech.com/open-source/litespeed-sapi/php).

Now that the OpenLiteSpeed server is installed, you will secure it by updating the default administrator account.

### 1.c. Setting the Administrative Password

Before testing the server, you will set a new administrative password for OpenLiteSpeed. You can do this by running a script provided by OpenLiteSpeed:

```
sudo /usr/local/lsws/admin/misc/admpass.sh
```

You will be asked to provide a username for the administrative user. If you press ENTER without choosing a new username, the default username admin will be used. You can use whatever administrative username you prefer. Then you will be prompted to create and confirm a new password for the account. Put in the administrative password you prefer, then press ENTER again. The script will confirm a successful update:

**Output**
```
Administrator's username/password is updated successfully!
```
You have now secured the admin account. Next, you will test the server to ensure it’s running properly.

### 1.d. Connecting to the Server

In this step, you will connect to your server.

OpenLiteSpeed should have started automatically after it was installed. You can verify if it started with the <code>systemctl status</code> command:

```
sudo systemctl status lsws
```

You will receive the following output:

**Output**
```cpp
● lshttpd.service - OpenLiteSpeed HTTP Server
     Loaded: loaded (/etc/systemd/system/lshttpd.service; enabled; vendor preset: enabled)
     Active: active (running) since Wed 2022-03-16 08:59:09 UTC; 2min 26s ago
    Process: 32997 ExecStart=/usr/local/lsws/bin/lswsctrl start (code=exited, status=0/SUCCESS)
   Main PID: 33035 (litespeed)
     CGroup: /system.slice/lshttpd.service
             ├─33035 openlitespeed (lshttpd - main)
             ├─33044 openlitespeed (lscgid)
             └─33073 openlitespeed (lshttpd - #01)
```
The active (running) message indicates that OpenLiteSpeed is running.

If your server is not running, you can start the server using systemctl:

```
sudo systemctl start lsws
```
The systemctl start command will print the following output:
**Output**
```
[OK] litespeed: pid=5137.
```
The server should now be running. Press <code>CTRL+C</code> to exit the service output.

Before visiting it in your browser, you will need to open some ports on your firewall, which you can achieve with the **ufw* command:

```
sudo ufw allow 8088,7080,443,80/tcp
```

The first port, 8088, is the default port for OpenLiteSpeed’s example site. After allowing it with ufw, it should now be accessible to the public. In your web browser, navigate to your server’s IP address or domain name, followed by :8088 to specify the port:

```
http://server_domain_or_IP:8088
```
Your browser will load the default OpenLiteSpeed web page, which will match the following image:

<img alt="Ubuntu" src="/Images/op-8088.png" />

You can look around the example website to explore the features offered by the web server.

To see the GUI-based Admin Panel, access port <code>7080</code>:


```
http://your_server_ip:7080
```
You will likely see a page warning you that the SSL certificate from the server cannot be validated. Because this is a self-signed certificate, this message is expected. Click through the available options to proceed to the site. In Chrome, you must click <code>Advanced</code> and then <code>Proceed to…</code> .

You will be prompted to enter the administrative username and password that you selected with the admpass.sh script in the previous step:

<img alt="Ubuntu" src="/Images/op-7080.png" />

Once authenticated, you will be presented with the OpenLiteSpeed administration interface:

<img alt="Ubuntu" src="/Images/op-admin-page.png" />

The majority of your configuration for the web server will take place via this dashboard.


### 2. Install some Utility packages

```
apt install -y git wget curl unzip nano zip
```

### 2.a. Mysql

The official Faveo installation uses Mysql as the database system and this is the only official system we support. While Laravel technically supports PostgreSQL and SQLite, we can’t guarantee that it will work fine with Faveo as we’ve never tested it. Feel free to read Laravel’s documentation on that topic if you feel adventurous.

Install Mysql 8.0 or MariaDB 10.6. Note that this only installs the package, but does not setup Mysql. This is done later in the instructions:

### For Ubuntu 18.04

```cpp
sudo apt update
sudo apt install software-properties-common -y
curl -LsS -O https://downloads.mariadb.com/MariaDB/mariadb_repo_setup
sudo bash mariadb_repo_setup --mariadb-server-version=10.6
sudo apt update
sudo apt install mariadb-server mariadb-client
sudo systemctl enable mariadb
```

### For Ubuntu 20.04

```cpp
sudo apt install dirmngr ca-certificates software-properties-common gnupg gnupg2 apt-transport-https curl -y
curl -fsSL http://repo.mysql.com/RPM-GPG-KEY-mysql-2022 | gpg --dearmor | sudo tee /usr/share/keyrings/mysql.gpg > /dev/null
echo 'deb [signed-by=/usr/share/keyrings/mysql.gpg] http://repo.mysql.com/apt/ubuntu focal mysql-8.0' | sudo tee -a /etc/apt/sources.list.d/mysql.list
echo 'deb-src [signed-by=/usr/share/keyrings/mysql.gpg] http://repo.mysql.com/apt/ubuntu focal mysql-8.0' | sudo tee -a /etc/apt/sources.list.d/mysql.list
sudo apt update
sudo apt install mysql-community-server -y
sudo systemctl start mysql
sudo systemctl enable mysql
```

### For Ubuntu 22.04

```
sudo apt update
sudo apt install mariadb-server mariadb-client -y
sudo systemctl start mariadb
sudo systemctl enable mariadb
```

Secure your MySql installation by executing the below command. Set Password for mysql root user, remove anonymous users, disallow remote root login, remove the test databases and finally reload the privilege tables.

```
mysql_secure_installation 
```
phpMyAdmin(Optional): Install phpMyAdmin. This is optional step. phpMyAdmin gives a GUI to access and work with Database

```
apt install phpmyadmin
```

### 2.b. Install wkhtmltopdf

Wkhtmltopdf is an open source simple and much effective command-line shell utility that enables user to convert any given HTML (Web Page) to PDF document or an image (jpg, png, etc).

It uses WebKit rendering layout engine to convert HTML pages to PDF document without losing the quality of the pages. Its is really very useful and trustworthy solution for creating and storing snapshots of web pages in real-time.

```
apt-get -y install wkhtmltopdf
```

### 3. Configuring LSPHP 8.1

Via **http://your_server_ip:7080**, log in to the Admin Panel (using the credentials you just set up) and navigate to the Server Configuration section. Then, click the External App tab.

You will see the following screen:

<img alt="Ubuntu" src="/Images/op-external-app.png" />

Here, you can configure your server to use any specific PHP processor. For this tutorial, we will use lsphp81.


- Replace <code>lsphp</code> with <code>lsphp81</code>
- Replace <code>uds://tmp/lshttpd/lsphp.sock</code> with <code>uds://tmp/lshttpd/lsphp81.sock</code>
- Replace <code>lsphp74/bin/lsphp</code> with <code>$SERVER_ROOT/lsphp81/bin/lsphp</code>

<img alt="Ubuntu" src="/Images/op-configure-php.png" />

Use the **Graceful Restart** button in the top right to restart the web server. The **Graceful Restart** button is highlighted in the upper right of the following screencapture:


<img alt="Ubuntu" src="/Images/op-gracefull-restart.png" />

Verify that your server is now using the specified PHP version by visiting the informational page at port <code>8088</code>:

```
http://your_server_ip:8088/phpinfo.php
```

The page will now display the specified version number.

In this step, you configured the credentials for the admin panel and set it to use the desired version of PHP. Next, you will set up Virtual Hosts for the different websites that you plan to host on this web server.

### 3.a Setting Up a Virtual Host

Moving next to configure the rewrite module which is an essential requirement for the WordPress features. Go to the Virtual Hosts and click on the view icon.

<img alt="Ubuntu" src="/Images/op-edit-virtual-host.png" />

Click on the **General** tab and edit the *General options* with the edit icon at the top right corner.

<img alt="Ubuntu" src="/Images/op-edit-virtual-host-general.png" />

In the **Document Root** field, type <code>$VH_ROOT/html/faveo</code> and click the save button at the top right corner.

<img alt="Ubuntu" src="/Images/op-virtual-host-general.png" />

Then again on the **General** tab of *Virtual Hosts* configuration, click the edit icon next to the *Index Files* section.

<img alt="Ubuntu" src="/Images/op-virtual-host-general-1.png" />

In the **Index Files** field, add *index.php* at the beginning of the section. Then click the save button at the top right corner.

<img alt="Ubuntu" src="/Images/op-virtual-host-index.png" />

Next, go to the **Rewrite tab** of the *Virtual Hosts* configuration view and edit the *Rewrite Control* options.

<img alt="Ubuntu" src="/Images/op-virtual-host-rewrites.png" />

Set **Enable Rewrite** and Auto Load from *.htaccess* to Yes and click the save icon at the top right corner.

<img alt="Ubuntu" src="/Images/op-rewrite-control.png" />

Once you’ve configured the OpenLiteSpeed server, Click the gracefully restart icon to apply the changes.


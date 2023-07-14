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

OpenLiteSpeed should have started automatically after it was installed. You can verify if it started with the **systemctl status** command:

```
sudo systemctl status lsws
```

You will receive the following output:

**Output**
``` 
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
The server should now be running. Press **CTRL+C** to exit the service output.

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

To see the GUI-based Admin Panel, access port **7080**:


```
http://your_server_ip:7080
```
You will likely see a page warning you that the SSL certificate from the server cannot be validated. Because this is a self-signed certificate, this message is expected. Click through the available options to proceed to the site. In Chrome, you must click **Advanced** and then **Proceed to…**.

You will be prompted to enter the administrative username and password that you selected with the admpass.sh script in the previous step:

<img alt="Ubuntu" src="/Images/op-7080.png" />

Once authenticated, you will be presented with the OpenLiteSpeed administration interface:

<img alt="Ubuntu" src="/Images/op-admin-page.png" />

The majority of your configuration for the web server will take place via this dashboard.


### 2. Install some Utility packages

```
apt install -y git wget curl unzip nano zip
```
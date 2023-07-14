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


## 1. LOMP Installation

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

### 1.b.  Installing OpenLiteSpeed

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
sudo apt install openlitespeed
```
If prompted, enter your password, then confirm the installation with Y.

Once the installation is complete, verify that OpenLiteSpeed is installed and working correctly by checking its status with the **service** command:

```
sudo systemctl status lsws
```
The **systemctl status** command obtains the status of a service identified by its keyword. The keyword for the OpenLiteSpeed Web Server service is **lsws**. The **systemctl** command can enable or disable automatic start for services and manually start or stop a service.

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

You now have an OpenLiteSpeed web server running with its default configuration. You may not be able to access the GUI-based Admin Panel and example website yet, as the firewall blocks traffic to these ports.

With your OpenLiteSpeed web server running, you can update the firewall and open the necessary ports to allow users to access the website.

### 1.b. Updating the Firewall

In this step, you will configure the firewall for your server. You will allow traffic over TCP to selected ports for the GUI-based admin panel and example website, as well as ports **80** and **443** for HTTP and HTTPS sites.

The OpenLiteSpeed server bundles a GUI-based admin panel and an example website with the server. The admin panel is an easy-to-use interface for configuring Listeners, Virtual Hosts, SSL, and monitoring logs. The example website features a sample CGI Script, PHP Script, Error Page, and a Password Protected Page. This website can demonstrate the capabilities of the web server.

The GUI-based Admin Panel listens on port **7080** in the default configuration, while the example website listens on port **8088**. You need to allow TCP traffic to these ports via the **ufw** firewall to access these sites.

To provide access, run the following command:

```
sudo ufw allow 7080,80,443,8088/tcp
```
Then, check the status of the firewall rules:

```
sudo ufw status
```
**Output**

```
Status: active

To                                Action      From
--                                ------      ----
OpenSSH                           ALLOW       Anywhere
80,443,7080,8088/tcp              ALLOW       Anywhere
OpenSSH (v6)                      ALLOW       Anywhere (v6)
80,443,7080,8088/tcp (v6)         ALLOW       Anywhere (v6)
```

You can view the example website through port 8088:

```
http://your_server_ip:8088
```
It should appear like the screencapture below:

<img alt="Ubuntu" src="/Images/op-8088.png" />

You can look around the example website to explore the features offered by the web server.

To see the GUI-based Admin Panel, access port **7080**:


```
http://your_server_ip:7080
```

Later in this tutorial, you will use the OpenLiteSpeed GUI Admin Panel to configure your web server.

You have now set up the OpenLiteSpeed server on your Ubuntu instance, which will allow you to serve a variety of web applications based on different back-end languages and frameworks. In the next step, you will set up other services of the LOMP stack.


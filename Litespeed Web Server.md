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


<img alt="Ubuntu" src="https://www.litespeedtech.com/images/logos/litespeed/litespeed-logo-square.png" width="120" height="120" />

Faveo can run on [Ubuntu 20.04 (Focal Fosa), Ubuntu 22.04 (Jammy Jellyfish)](http://releases.ubuntu.com/22.04/).

- [<strong>Installation steps :</strong>](#installation-steps-)
    - [<strong>1. LLMP Installation</strong>](#1-stack-installation)
    - [<strong>2. Install some Utility packages</strong>](#2-install-some-utility-packages)


<a id="installation-steps-" name="installation-steps-"></a>

<a id="1-stack-installation" name="litespeed-stack-install-"></a>

## 1. LLMP Installation

<p>Follow the instructions here If you follow this step, no need to install Litespeed, PHP, MySQL separetely as listed below</p>

Run the following commands as sudoers or Login as root user by typing the command below 

```
sudo su
```
### 1.a. Update your package list

```
apt update && apt upgrade -y
```


### 1.b. Litespeed

<p>Open port 7080, but only to your IP address</p>
<p>Use the following UFW command which will allow access to port 7080 only to the IP specified.</p>

```
sudo ufw allow from YOUR_IP to any port 7080 proto tcp
```

### Download LS to your server:

Download the LiteSpeed Web Server tarball from the [LiteSpeed Technologies website](http://www.litespeedtech.com/download/litespeed-web-server-download?_gl=1*bxqlqf*_gcl_au*MTkyNzY4NzMxNi4xNjg4OTc5OTI5).

<p>Run the following commands from SSH as root:</p>

```
cd /root
wget http://www.litespeedtech.com/packages/6.0/lsws-6.0.6-ent-x86_64-linux.tar.gz
```
### Unpack

Use the following command to unpack the tarball:

```
tar zxfv  lsws-*-ent-x86_64-linux.tar.g
```
### Run Installation Script

Access the unpacked folder:
```
cd lsws-*
```

Create a serial.no file containing your paid license serial number or your trial key:

```
echo "YOUR_SERIAL_NO" > serial.no
```

Replace YOUR_SERIAL_NO with your actual license serial number or trial key. (Double quotes are optional in this context.)

Run the install script:

```
./install.sh
```
The installer will ask you a number of questions covered in detail [here](http://www.litespeedtech.com/docs/webserver/install?_gl=1*fgcmxe*_gcl_au*MTkyNzY4NzMxNi4xNjg4OTc5OTI5).

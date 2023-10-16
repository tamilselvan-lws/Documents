### Download XAMPP:

- Download the latest version of XAMPP for your Windows operating system.
[Click here](https://sourceforge.net/projects/xampp/files/XAMPP%20Windows/8.1.17/xampp-windows-x64-8.1.17-0-VS16-installer.exe/download)

### Upload Faveo
- Download the Faveo Helpdesk from https://billing.faveohelpdesk.com and upload it to the below directory.

```
C:\xampp\htdocs\faveo
```

### Install Ioncube Loader

[Click Here](https://downloads.ioncube.com/loader_downloads/ioncube_loaders_win_vc16_x86-64.zip) to download IonCube Loader zip file.

- Step 1: Extract the IonCube Loader file downloaded.

- Step 2: Copy the <code><b>ioncube_loader_win_8.1.dll</b></code> file and paste it into the PHP extensions directory <code><b>C:\xampp\php\ext</b></code>.

- Step 3: Copy the “loader-wizard.php” from the extracted Ioncube folder and paste it into the <code><b>C:\xampp\htdocs</b></code>.

- Step 4: Edit the <code><b>C:\xampp\php\php.ini</b></code> file and below the last line enter the path to the extension within the <code><b>zend_extension</b></code> parameter:

```
zend_extension = "C:\xampp\php\ext\ioncube_loader_win_8.1.dll"
```
- Step 5: Run the below URL to verify the ionCube Installation.
- Note: If you didn’t get the below output try restarting the Apache Server.

```
http://127.0.0.1\loader-wizard.php
```

<img src="https://raw.githubusercontent.com/ladybirdweb/faveo-server-images/master/_docs/installation/providers/enterprise/windows-images/apache32.png" alt="" style=" width:500px ; height:150px ">

### Download & Enable cacert.pem File in PHP Configuration File


-   <a href="https://www.faveohelpdesk.com/user-manual/windows_installation/pem_file.zip)" target="_blank" rel="noopener">Click Here</a> to download <code><b>cacart.pem</b></code> file. This is required to avoid the “cURL 60 error” which is one of the Probes that Faveo checks.
- Extract the <code><b>cacert.pem</b></code> file and copy it to <code><b>C:\xampp\php</b></code> path.
- Edit the *php.ini*, Uncomment *curl.cainfo* and add the location of cacert.pem to it as below:
```
curl.cainfo = "C:\xampp\php\cacert.pem"
```


### Configure the PHP 8.1

- Step 1: Enable the extensions listed below, by uncommenting them (Remove the semicolon **;** at beginning of line).

Default Extensions

```
extension=ldap
extension=gd
extension=imap
extension=soap
extension=sockets
extension=sodium
```

- Step 2: Set the recommended minimum value of these PHP directives listed below for Faveo to work properly. 
- Search for the directives and assign the value according to this example:

```
max_execution_time = 360
max_input_time = 360
max_input_vars = 10000
memory_limit = 256M
post_max_size = 1024M
upload_max_filesize = 100M
max_file_uploads = 100
short_open_tag = On
```

### Install Redis Extension

<a href="https://pecl.php.net/package/redis/5.3.7/windows" target="_blank" rel="noopener">Click Here</a> to download PHP 8.1 Thread Safe (TS) x64 zip file.

<img src="https://raw.githubusercontent.com/ladybirdweb/faveo-server-images/master/_docs/installation/providers/enterprise/windows-images/PHPredis.png" style=" width:400px ; height:250px ">

- Unzip the php-redis zip file, a folder will be created, go inside the folder, copy the <code><b>php_redis.dll</b></code> file and paste it in <code><b>C:\xampp\php\ext</b></code>. (C:\xampp\php incase of Apache WebServer).
- Now enable php redis extension in <code><b>php.ini</b></code> configuration located in <code><b>C:\php8.1.</b></code>  *(C:\xampp\php incase of Apache WebServer).*

```
extension=php_redis.dll
```

### Self-Signed SSL

This document will list how to install Self-Signed SSL certificates on Windows servers.

- We will be using the tool OpenSSL for creating a Self-Signed SSL certificate on a windows machine.

- The OpenSSL is an open-source library that provides cryptographic functions and implementations. 

- OpenSSL is a defacto library for cryptography-related operations and is used by a lot of different applications. 

- OpenSSL is provided as a library and application. 

- OpenSSL provides functions and features like SSL/TLS, SHA1, Encryption, Decryption, AES, etc.

Before proceeding with the SSL installation Load the following modules for SSL in httpd.conf
```
LoadModule ssl_module modules/mod_ssl.so
Include conf/extra/httpd-ssl.conf
LoadModule socache_shmcb_module modules/mod_socache_shmcb.so
```

## Installing Chocolatey and OpenSSL on Windows

### Step 1: Install Chocolatey

- Open an elevated Command Prompt or PowerShell. Right-click on the "Start" button and select "Windows Terminal (Admin)" or "Command Prompt (Admin)" or "Windows PowerShell (Admin)".

- Copy and paste the following command to download and run the Chocolatey installation script:

```
Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

- You may be prompted to confirm the execution of scripts. Enter "A" and press Enter to allow the script to run.

- Wait for the installation to complete. Chocolatey will be installed in the "C:\ProgramData\chocolatey" directory by default.

- To verify the installation, open a new Command Prompt or PowerShell window and run:

```
choco --version
```

### Install OpenSSL

- After successfully installing Chocolatey, you can proceed to install OpenSSL.

- Open a new Command Prompt or PowerShell window with administrator privileges.

- To install OpenSSL, run the following Chocolatey command:

```
choco install openssl
```

- Follow the on-screen prompts to confirm the installation. You can typically accept the default options by pressing Enter.

- Wait for the installation to complete. Chocolatey will download and install OpenSSL and its dependencies.

- To verify the OpenSSL installation, run: 
```
openssl version
```

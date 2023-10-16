### Download Wamp:

- Download the latest version of WampServer for your Windows operating system.
[Click here](https://sourceforge.net/projects/wampserver/files/WampServer%203/WampServer%203.0.0/wampserver3.3.0_x64.exe/download)

### Upload Faveo
- Download the Faveo Helpdesk from https://billing.faveohelpdesk.com and upload it to the below directory.

```
C:\wamp64\www
```

### Install Ioncube Loader

[Click Here](https://downloads.ioncube.com/loader_downloads/ioncube_loaders_win_vc16_x86-64.zip) to download IonCube Loader zip file.

- Step 1: Extract the IonCube Loader file downloaded.

- Step 2: Copy the <code><b>ioncube_loader_win_8.1.dll</b></code> file and paste it into the PHP extensions directory <code><b>C:\wamp64\bin\php\php8.1.13\ext</b></code>.

- Step 3: Copy the “loader-wizard.php” from the extracted Ioncube folder and paste it into the <code><b>C:\wamp64\www</b></code>.

- Step 4: Edit the <code><b>C:\wamp64\bin\php\php8.1.13\php.ini</b></code> file and below the last line enter the path to the extension within the <code><b>zend_extension</b></code> parameter:

```
zend_extension = "C:\wamp64\bin\php\php8.1.13\ext\ioncube_loader_win_8.1.dll"
```
- Step 5: Run the below URL to verify the ionCube Installation.
- Note: If you didn’t get the below output try restarting the Apache Server.

```
http://127.0.0.1\loader-wizard.php
```

<img src="https://raw.githubusercontent.com/ladybirdweb/faveo-server-images/master/_docs/installation/providers/enterprise/windows-images/apache32.png" alt="" style=" width:500px ; height:150px ">


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

- Unzip the php-redis zip file, a folder will be created, go inside the folder, copy the <code><b>php_redis.dll</b></code> file and paste it in <code><b>C:\wamp64\bin\php\php8.1.13\ext</b></code>. (C:\xampp\php incase of Apache WebServer).
- Now enable php redis extension in <code><b>php.ini</b></code> configuration located in <code><b>C:\wamp64\bin\php\php8.1.13</b></code>.  

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

---

- Edit the <code><b>httpd.conf</b></code> file Open the file <code><b>C:\wamp64\bin\apache\apache2.4.54.2\conf\httpd.conf</b></code> and un-comment (remove #) from the following 3 lines 

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

- You may be prompted to confirm the execution of scripts. Enter <code><b>"A"</b></code> and press Enter to allow the script to run.

- Wait for the installation to complete. Chocolatey will be installed in the <code><b>"C:\ProgramData\chocolatey"</b></code> directory by default.

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
### Steps

- Generate a private key for the CA (Certificate Authority).
- Generate a certificate signing request for the CA.
- Generate a root certificate.
- Create a private key for the certificate.
- Create a certificate signing request.
- Create a certificate and sign it with the CA private key.
- Compiling the created certificate and key file as a .pfx file.
- Installing the SSL certificate.

### Generate a private key for the CA

Create a directory named SSL under C directory like <code><b>C:\SSL</b></code>, the following commands will create the SSL files those files will be saved in the directory which we create.

- Open Command Prompt from the SSL directory that we created,

- Run the below command to create a Private key for the rootCA this command will save a file name faveoroot.key in the SSL folder.

```
openssl ecparam -out faveoroot.key -name prime256v1 -genkey
```

### Generate a certificate signing request for the CA

From the command prompt run the below command which will create a CSR (certificate signing request) for the Root CA.

```
openssl req -new -sha256 -key faveoroot.key -out faveoroot.csr
```

The above command will ask for the below information if needed you can provide them or you can just hit enter and skip them but it is recommended to give the meaningful details.

- Country Name.
- State Name.
- Organization.
- Common name (Leave this as blank or provide the company domain not the faveo domain).
- Email address.

The above command will save a file in the name faveoroot.csr in the SSL directory.

### Generate a root certificate

The below command will create the Root CA certificate which we will use to sign the SSL certificates. 

```
openssl x509 -req -sha256 -days 3650 -in faveoroot.csr -signkey faveoroot.key -out faveorootCA.crt
```

The above command will create a file and save it as faveorootCA.crt in the SSL directory.

### Create a private key for the certificate 

The below command will create a private key file for the server SSL certificate.

```
openssl ecparam -out private.key -name prime256v1 -genkey
```

The above command will save a key file with the name private.key for the server SSL certificate.

### Create a certificate signing request for the server SSL

The below command will create a Certificate Signing Request for the Server SSL.

```
openssl req -new -sha256 -key private.key -out faveolocal.csr
```

It will ask for the details below we should give the details as shown below.

- Country Name.
- State Name.
- Organization.
- Common name (Here please provide the Domain or the IP through which you need to access faveo).
- Email address.

The rest can be left blank and after this is completed it will create the CSR file and save it with the name faveolocal.csr in the SSL directory.

### Create a certificate and sign it with the CA private key

The below command will create the server SSL certificate which is signed by the Root CA that we created above.

```
openssl x509 -req -in faveolocal.csr -CA  faveorootCA.crt -CAkey faveoroot.key -CAcreateserial -out faveolocal.crt -days 3650 -sha256 
```

The above command will create a server SSL file and save it in the name faveolocal.crt, this certificate will be valid for 3650 days which is ten years.

### Verify the newly created certificate

```
openssl x509 -in faveolocal.crt -text -noout  
```

### Compiling the created certificate and key file as .pfx file

As windows need the certificate file in .pfx format which will contain the both certificate and the key file and the CA file for the installation, so we need to convert the created files to .pfx format, this can be done with the below command.
```
openssl pkcs12 -export -out cert.pfx -inkey private.key -in faveolocal.crt -certfile faveorootCA.crt
```
The above command will create a .pfx file with the name cert.pfx in the SSL directory.


### Download & Enable cacert.pem File in PHP Configuration File


-   <a href="https://curl.se/docs/caextract.html)" target="_blank" rel="noopener">Click Here</a> to download <code><b>cacart.pem</b></code> file. This is required to avoid the “cURL 60 error” which is one of the Probes that Faveo checks.
- Extract the <code><b>cacert.pem</b></code> file and copy it to <code><b>C:\wamp64\bin\php\php8.1.13</b></code> path.
- Edit the *php.ini*, Uncomment *curl.cainfo* and add the location of cacert.pem to it as below:
```
curl.cainfo = "C:\wamp64\bin\php\php8.1.13\cacert.pem"
```

- Edit the faveorootCA.crt and copy the content.

----images----

- Edit the cacert.pem file and append the content copied from faveorootCA.crt in this file.

----images----

- Save and Close the File.

### Edit the “hosts” file on the OS to Map the Custom Domain to Loopback Address.

- Open the hosts file from the path <code><b>C:\Windows\System32\drivers\etc</code>/<b>

---image -----

- Map your custom domain to 127.0.0.1

---images-----

- Save and Close the File.

- if the above is done we need to edit the php.ini file which is found inside the PHP root directory. Uncomment and add the location of <code><b>cacert.pem</code></b> to <code><b>“openssl.cafile”</code></b> like.
```
openssl.cafile = "C:\php\cacert.pem"
```

- Edit the <code><b>C:\Apache24\conf\extra\httpd-ssl.conf</code></b> file, search for <VirtualHost _default_:443>
- Turn SSL Engine on & add the certificate paths respectively as shown below:

```
SSLEngine on
SSLCertificateFile "C:\SSL\faveolocal.crt"
SSLCertificateKeyFile "C:\SSL\private.key"
SSLCACertificateFile "C:\SSL\faveorootCA.crt"
```


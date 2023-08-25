---
layout: single
type: docs
permalink: /docs/installation/providers/enterprise/redis-windows/
redirect_from:
  - /theme-setup/
last_modified_at: 2023-08-25
last_modified_by: TamilSelvan_M
toc: true
title: Installing Redis on Windows Server
---


<img alt="Windows" src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/e2/Windows_logo_and_wordmark_-_2021.svg/250px-Windows_logo_and_wordmark_-_2021.svg.png" width="200"  />   


<img alt="Windows" src="https://upload.wikimedia.org/wikipedia/en/6/6b/Redis_Logo.svg" width="200"  /> 

[<strong>Install Redis & Configure it with WinSW</strong>](#Install-Redis-&-Configure-it-with-WinSW)


<b>1. Install PHP 8.1 </b>

<a href="https://pecl.php.net/package/redis/5.3.7/windows" target="_blank" rel="noopener">Click Here</a> to download PHP 8.1 NTS x64 zip file.

<img src="https://raw.githubusercontent.com/ladybirdweb/faveo-server-images/master/_docs/installation/providers/enterprise/windows-images/PHPredis.png" style=" width:400px ; height:250px ">

- Unzip the php-redis zip file, a folder will be created, go inside the folder, copy the *php_redis.dll* file and paste it in *C:\php8.1\ext.* *(C:\php\ext incase of Apache WebServer).* 
- Now enable php redis extension in *php.ini* configuration located in *C:\php8.1.*  *(C:\php incase of Apache WebServer).*

```
extension=php_redis.dll
```
- Now go to Server Manager, open IIS Server and restart it. *(or restart Apache incase of Apache WebServer)*



<b>2. Install Redis </b>

<a href="https://github.com/tporadowski/redis/releases" target="_blank" rel="noopener">Click Here</a> to download last recent release of Redis.

<img src="https://github.com/ladybirdweb/faveo-server-images/blob/master/_docs/installation/providers/enterprise/windows-images/redis2.png?raw=true" style=" width:400px ; height:250px ">

Execute the installer to perform the required installation steps
- Accept the terms in the License agreement.
- Add the Redis installation folder to the PATH environment variable.

<img src="https://github.com/ladybirdweb/faveo-server-images/blob/master/_docs/installation/providers/enterprise/windows-images/redis3.png?raw=true" style=" width:400px ; height:250px ">

- Set the Max Memory Limit from 300-500 MB.

<img src="https://github.com/ladybirdweb/faveo-server-images/blob/master/_docs/installation/providers/enterprise/windows-images/redis4.png?raw=true" style=" width:400px ; height:250px ">

The Redis installation is finished. 

- Now go to *windows menu > Run > Services*, This can be done by  following shortcut *win+R*, type *services.msc* & click *OK*, it will open a new Services tab. 
- Find *Redis* right click on it & go to *Properties > Recovery* a new tab will open. 

<img src="https://github.com/ladybirdweb/faveo-server-images/blob/master/_docs/installation/providers/enterprise/windows-images/redis5.png?raw=true" style=" width:400px ; height:250px ">

- Change *first failure, second failure & subsequent failure* from Take No Action to *Restart the Service* & click *OK*. 

<img src="https://github.com/ladybirdweb/faveo-server-images/blob/master/_docs/installation/providers/enterprise/windows-images/redis6.png?raw=true" style=" width:400px ; height:250px ">

Redis has been successfully installed, this can be confirmed in Command Prompt by typing *redis-cli*, a loopback address will be shown in the Command Prompt.

## Install WinSW Service

Download WinSW Config File.
- [IIS](https://raw.githubusercontent.com/tamilselvan-lws/Documents/main/Redis%20Windows%20Server/Files/WinSW-IIS.zip) 
- [Apache](https://raw.githubusercontent.com/tamilselvan-lws/Documents/main/Redis%20Windows%20Server/Files/WinSW-Apache.zip)

Unzip the <code>WinSW-{version}.zip</code> file.

Go inside the folder & copy the WinSW folder & paste it in C:\ .

<img src="/Redis Windows Server/images/Redis-dir.png" style=" width:400px">

<b>4. General Instructions to Create WinSW Services</b>

**NOTE: These are the General Instructions to create WinSW Services for Faveo. The actual ones are listed below as Faveo-Mail-Worker, Faveo-Recurring, Faveo-Reports & Faveo-Notifications.**

Go to Command Prompt and configure WinSW-SERVICE by typing the following commands. (Faveo-Mail-Worker, Faveo-Recurring, Faveo-Reports & Faveo-Notifications are to be configured in actual).


- Install your application as a service using the following command:

```
winsw install
```

**Follow the above general steps to create WinSW Services for FAVEO MAIL WORKER, FAVEO RECURRING, FAVEO REPORTS & FAVEO NOTIFICATIONS**

- **FAVEO WORKER**

Open a Command Prompt window with administrator privileges.

Navigate to the directory containing the WinSW executable and your application files using the cd command. For example:

```
cd C:\WinSW\Faveo-Mail-Worker
```
Install your application as a service using the following command:

```
winsw install
```

Start the service:

```
winsw start
```


- **FAVEO RECURRING**

Open a Command Prompt window with administrator privileges.

Navigate to the directory containing the WinSW executable and your application files using the cd command. For example:

```
cd C:\WinSW\Faveo-Recurring
```
Install your application as a service using the following command:

```
winsw install
```

Start the service:

```
winsw start
```

- **FAVEO REPORTS**

Open a Command Prompt window with administrator privileges.

Navigate to the directory containing the WinSW executable and your application files using the cd command. For example:

```
cd C:\WinSW\Faveo-Reports
```
Install your application as a service using the following command:

```
winsw install
```

Start the service:

```
winsw start
```


- **FAVEO NOTIFICATIONS**

Open a Command Prompt window with administrator privileges.

Navigate to the directory containing the WinSW executable and your application files using the cd command. For example:

```
cd C:\WinSW\Faveo-Notifications
```
Install your application as a service using the following command:

```
winsw install
```

Start the service:

```
winsw start
```


- **FAVEO DEACTIVATYE AGENTS**

Open a Command Prompt window with administrator privileges.

Navigate to the directory containing the WinSW executable and your application files using the cd command. For example:

```
cd C:\WinSW\Faveo-Deactivation
```
Install your application as a service using the following command:

```
winsw install
```

Start the service:

```
winsw start
```

Open service manager and check status of all the services configured by WinSW.

<img src="/Redis Windows Server/images/Redis-Status.png" style=" width:400px">
<img alt="Ubuntu" src="/Images/openlitespeed_logo_grey_bold.png" height="120" />

## Introduction

This document will list steps on how to install Redis, Supervisor and Worker for faveo.

We will install following dependencies in order to make Redis work:

- Redis
- LSPHP extension for Redis
- supervisor

Switch to root user or run the following commands as sudoers.

```
sudo su
```
### PHP 8.1+

First add this PPA repository:

```
add-apt-repository ppa:ondrej/php
```

Then install php 8.1 with these extensions:

```
apt update
apt install -y php8.1 libapache2-mod-php8.1 php8.1-mysql \
    php8.1-cli php8.1-common php8.1-fpm php8.1-soap php8.1-gd \
    php8.1-opcache  php8.1-mbstring php8.1-zip \
    php8.1-bcmath php8.1-intl php8.1-xml php8.1-curl  \
    php8.1-imap php8.1-ldap php8.1-gmp php8.1-redis
```

### Install Redis and LSPHP redis extension

```
sudo apt install redis lsphp81-redis
```
### Start, Enable and restart the Redis-service

```
systemctl start redis-server
systemctl enable redis-server
```
### Install and Configure Supervisor

```
apt-get install supervisor
```
### Copy paste the below configuration.( Change the directories according to your configuration)

```
nano /etc/supervisor/conf.d/faveo-worker.conf
```

```cpp
[program:faveo-Horizon]
process_name=%(program_name)s
command=php /usr/local/lsws/Example/html/faveo/artisan horizon
autostart=true
autorestart=true
user=www-data
redirect_stderr=true
stdout_logfile=/usr/local/lsws/Example/html/faveo/storage/logs/horizon-worker.log
```

### Restart the Supervisor to reread the new configuration.Permalink
```
systemctl restart supervisor 
```
To check the Status of workers use the below command

```
supervisorctl
```

### Enable Redis in FaveoPermalink
After Redis installation is complete, follow these [instructions](https://docs.faveohelpdesk.com/docs/helper/enable-redis) to configure Redis with Faveo.
---
layout: single
type: docs
permalink: /docs/installation/providers/enterprise/self-signed-ssl-rhel-nginx/
redirect_from:
  - /theme-setup/
last_modified_at: 2023-11-06
toc: true
title: Install Self-Signed SSL for Faveo on RHEL 9

---

## Introduction
This document will guide on how to install Self-Signed SSL certificates on RHEL 9 with nginx.

## Setting up the SSL certificate
To Install Self Signed SSL certificates in RHEL 9, We need to create SSL Cetificates which is signed by the CA certificate, after that we need to add the Virtual host file for the SSL certificate and edit the php.ini file and the hosts file the steps are explained below.

## <strong>Steps</strong>

- Generate a private key for the CA (Certificate Authority).
- Generate a certificate signing request for the CA.
- Generate a root certificate.
- Create a private key for the certificate.
- Create a certificate signing request.
- Create a certificate and sign it with the CA private key.
- Installing the SSL certificate.

### <strong>Generate a private key for the CA</strong>

Create a directory named SSL in the home or on any directory, the following commands will create the SSL files those files will be saved in the directory which we create.
- From the SSL folder that was created run the below commands.
- Run the below command to create a Private key for the rootCA this command will save a file name faveoroot.key in the SSL folder.

```
openssl ecparam -out faveoroot.key -name prime256v1 -genkey
```

### <strong>Generate a certificate signing request for the CA</strong>

- Run the below command which will create a CSR (certificate signing request) for the Root CA.

```
openssl req -new -sha256 -key faveoroot.key -out faveoroot.csr
```
- The above command will ask for the below information if needed you can provide them or you can just hit enter and skip them but it is recommended to give the meaningful details.

    - Country Name.
    - State Name.
    - Organization.
    - Comman name (Leave this as blank or provide the company domain not the faveo domain)
    - Email address.

- The above command will save a file in the name faveoroot.csr in the SSL directory.

### <strong>Generate a root certificate</strong>

- The below ccommand will create the Root CA certificate which we will use to sign the SSL certificates.


```
openssl x509 -req -sha256 -days 3650 -in faveoroot.csr -signkey faveoroot.key -out faveorootCA.crt
```

- The above command will create a file and save it as faveorootCA.crt in the SSL directory.

### <strong>Create a private key for the certificate</strong>

- The below command will create a private key file for the server SSL certificate.

```
openssl ecparam -out private.key -name prime256v1 -genkey
```

- The above command will save a key file with the name private.key for the server SSL certificate.

### <strong>Create a certificate signing request for the server SSL</strong>

- The below command will create a Certificate Signing Request for the Server webpage SSL.

```
openssl req -new -sha256 -key private.key -out faveolocal.csr
```

- It will ask for the details as below we should give the details as shown below.

    - Country Name.
    - State Name.
    - Organization.
    - Common name (Here please provide the Domain or the IP through which you need to access faveo).
    - Email address.
- The rest can be left blank and after this is completed it will create the CSR file and save it with the name faveolocal.csr in the SSL directory.

### <strong>Create a certificate and sign it with the CA private key</strong>

- The below command will create the server SSL certificate which is signed by the Root CA that we created above.

```
openssl x509 -req -in faveolocal.csr -CA  faveorootCA.crt -CAkey faveoroot.key -CAcreateserial -out faveolocal.crt -days 3650 -sha256 
```
- The above command will create a server SSL file and save it in the name faveolocal.crt, this certificate will be valid for 3650 days that is ten years.

## Setting up the Virtual host file for the Self signed SSL certificate's.

- Before creating the Virtual host file for SSL we need to copy the created SSL certificate's and Key file to the corresponding directory with below command, these commands should be runned from the SSL Directory.
```
cp faveolocal.crt /etc/pki/tls/certs
cp private.key /etc/pki/tls/private
cp faveorootCA.crt /etc/pki/ca-trust/source/anchors/
```
- Then adding the Virtual host file, for that we need to create a file in webserver directory as <b> nano /etc/nginx/nginx.conf.</b>
- Add the following lines to your Nginx configuration, modifying the file paths as needed:

```
    listen 443 ssl;
    ssl_certificate /etc/ssl/certs/faveolocal.crt; 
    ssl_certificate_key /etc/pki/tls/private/private.key; 
```

```
user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

# Load dynamic modules. See /usr/share/doc/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 4096;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    # Load modular configuration files from the /etc/nginx/conf.d directory.
    # See http://nginx.org/en/docs/ngx_core_module.html#include
    # for more information.
    include /etc/nginx/conf.d/*.conf;

    server {
        server_name  ---DomainName or IP---;
        root         /var/www/faveo/public/;
        index index.php index.html index.htm;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;
#This is for user friendly URL 
    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~* \.html$ {
        expires -1;
    }

    location ~* \.(css|gif|jpe?g|png)$ {
        expires 1M;
        add_header Pragma public;
        add_header Cache-Control "public, must-revalidate, proxy-revalidate";
    }

     	error_page 404 /404.html;
        location = /404.html {
        }

        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
        }

    listen 443 ssl;
    ssl_certificate /etc/ssl/certs/faveolocal.crt; 
    ssl_certificate_key /etc/pki/tls/private/private.key; 
}

    gzip on;
    gzip_http_version 1.1;
    gzip_vary on;
    gzip_comp_level 6;
    gzip_proxied any;
    gzip_types application/atom+xml
           application/javascript
           application/json
           application/vnd.ms-fontobject
           application/x-font-ttf
           application/x-web-app-manifest+json
           application/xhtml+xml
           application/xml
           font/opentype
           image/svg+xml
           image/x-icon
           text/css
           #text/html -- text/html is gzipped by default by nginx
           text/plain
           text/xml;
    gzip_buffers 16 8k;
    gzip_disable "MSIE [1-6]\.(?!.*SV1)";   
}
```

## After Creating the Virtual Host file we need to add the local host for the domain.

- Need to update the CA certificate's run the below command to update the CA certs.
```
update-ca-trust extract
```

- After adding the SSL certificates and virtual hosts we need to add the domain to the hosts file to the local host as below.
```
nano /etc/hosts
```
- In the above file add the below line replace the domain or the IP which is used for the faveo.
```
127.0.0.1  ---Domain or IP---
```
- After the above is done then we need to add the the ca-cert file path to the <b>/etc/php.ini</b> file add the path to the openssl.cafile like this : 

```
openssl.cafile = "/etc/pki/tls/certs/ca-bundle.crt"
```

```
systemctl restart php-fpm.service
systemctl restart nginx
```

- Now check the faveo on the Browser it will take you to probe page, if everything is good then you can proceed with the installation in Browser.

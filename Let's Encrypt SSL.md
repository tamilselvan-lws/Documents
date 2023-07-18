
<img alt="Ubuntu" src="/Images/openlitespeed_logo_grey_bold.png" height="120" />

## Introduction

This document will list on how to install LetsEncrypt SSL on Ubuntu Running Openlitespeed Web Server.

PS : Please replace example.com with your valid domain name which is mapped with your server

We will install following dependencies in order to make LetsEncrypt SSL work:

- certbot

### Downloading the LetsEncrypt client for Ubuntu

```
sudo apt update
sudo apt install certbot
```

### Setting up the SSL certificate

Certbot will handle the SSL certificate management quite easily, it will generate a new certificate for provided domain as a parameter.

In this case, example.com will be used as the domain for which the certificate will be issued:

```
certbot certonly --webroot -w /usr/local/lsws/Example/html/faveo -d example.com
```

You will then be prompted to answer the following questions.

- Enter Email address: Type in your email address
- Accept the terms of service: A
- Share your Email Address with EFF: Type Y for yes and N for No.
- Enter Domain name: Type your FQDN (fully qualified domain name) here
- Input the Web root: /usr/local/lsws/Example/html/faveo/

Once you have answered all the questions and validation process is complete, the certificate files will be saved in /etc/letsencrypt/live/**example.com**/ directory

**Output**

```cpp
Successfully received certificate.
Certificate is saved at: /etc/letsencrypt/live/example.com/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/example.com/privkey.pem

```

Next, configure the WordPress site on your OpenLiteSpeed server to use the SSL certificate. Navigate to the Virtual Host configuration and open the SSL tab. Edit the SSL Private Key & Certificate.

<img alt="Ubuntu" src="/Images/op-add-ssl-keys.png" />



<img alt="Ubuntu" src="/Images/op-ssl-keys.png" />

Once completed, go to Listeners and add a new listener.

<img alt="Ubuntu" src="/Images/op-add-listener.png" />

Fill in the fields as follows:

```
Listener Name: SSL
IP Address: ANY
Post: 443
Binding:
Enable REUSEPORT: Not Set
Secure: Yes
```

Once all set, apply the new settings by clicking the save icon on the right.

<img alt="Ubuntu" src="/Images/op-listener-enable-secure.png" />

Next, view the SSL listener to configure the Virtual host mapping.

<img alt="Ubuntu" src="/Images/op-ssl-listener.png" />

Add a row in Virtual Host Mappings.

<img alt="Ubuntu" src="/Images/op-add-virtual-host.png" />

Choose the virtual host and type in your domain name. Save the settings from the save button on the top right corner.

<img alt="Ubuntu" src="/Images/op-virtual-host-domains.png" />

Once you’ve configured the SSL with your OpenLiteSpeed server, click the gracefully restart icon to apply the changes.


### Setting up auto renewal of the certificate

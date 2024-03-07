# Wazuh server 

1. Download the Wazuh installation assistant.

```
sudo apt update && sudo apt install curl apt-transport-https unzip wget libcap2-bin software-properties-common lsb-release gnupg2
```

```
curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh && chmod 744 wazuh-install.sh && bash ./wazuh-install.sh -a
```

Output:

```
27/02/2024 09:07:17 INFO: You can access the web interface https://<wazuh-dashboard-ip>:443
    User: admin
    Password: yLdqpFL2q*asiwcrZXoVx01V2hlxxChP
```

https://chat.googleapis.com/v1/spaces/AAAAlfJfAig/messages?key=AIzaSyDdI0hCZtE6vySjMm-WEfRq3CPzqKqqsHI&token=IMbt-4ETupl614Sa1vj7M45b2gkak-vQrdR7pHW3RBI

centralized login
notification



rsync



212.2.247.61

<rule id="100100" level="7">
    <if_sid>550</if_sid>
    <match>alert</match>
    <options>no_repeat_alert</options>
    <description>webhook</description>
    <group>webhook</group>
    <user>www-data</user>
    <command>curl -X POST -d "message=Wazuh alert" https://chat.googleapis.com/v1/spaces/AAAAlfJfAig/messages?key=AIzaSyDdI0hCZtE6vySjMm-WEfRq3CPzqKqqsHI&token=IMbt-4ETupl614Sa1vj7M45b2gkak-vQrdR7pHW3RBI</command>
</rule>

<integration>
  <name>chatbot</name>
  <hook_url>https://chat.googleapis.com/v1/spaces/AAAAlfJfAig/messages?key=AIzaSyDdI0hCZtE6vySjMm-WEfRq3CPzqKqqsHI&token=IMbt-4ETupl614Sa1vj7M45b2gkak-vQrdR7pHW3RBI</hook_url>
  <level>3</level>
  <alert_format>json</alert_format>
</integration>


relayhost = [smtp.gmail.com]:587
smtp_sasl_auth_enable = yes
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
smtp_sasl_security_options = noanonymous
smtp_tls_CAfile = /etc/ssl/certs/ca-certificates.crt
smtp_use_tls = yes
smtpd_relay_restrictions = permit_mynetworks, permit_sasl_authenticated, defer_unauth_destination



echo [smtp.gmail.com]:587 cofigureemail@gmail.com:zxnagcgaklzofzli > /etc/postfix/sasl_passwd


echo "Test mail from postfix" | mail -s "Test Postfix" -r "tamil.selvan@ladybirdweb.com" tamil.selvan@ladybirdweb.com





Client has confirmed for tomorrow at 8:30am , below are the queries which they have 



Few questions we would like to answers for now are:
Extension gd2 (as per instruction manual)

gd2 for png and jpg

is “Redis” optional “ what are cons not using it?
not optional

is “MEMcached” optional “ what are cons not using it?
skip 

Is “Meilisearch” optional “ what are cons not using it?
it improve the ticket search ..


Do you have offline update process ?
 
Note: we have installed on Windows Server 2022.


## 2 installations RHEL & Debian
1. Faveo installation on both.
2. Apache & Nginx Hardening.
3. Mysql  & MariaDB Optimization
4. One Server mysql another mariadb
5. Install Webmin
6. Configure webmin csf
7. Zabbix server, agent : one with server and other with agent
8. Install AWX Ansible on one server and connect another server

## Server


### 1. Hostname: TS-Debian12
``` 
   IP: 65.108.243.176
   Domain: https://debian.thetamilselvan.in
```
### 2. Hostname: TS-Rocky9
```
   IP: 212.2.241.32
   Domain: https://rocky.thetamilselvan.in
```

#### Add lwsadmin User and root enable (Rocky Linux)
```
adduser lwsadmin
passwd lwsadmin
usermod -aG wheel lwsadmin
```

nano /etc/ssh/sshd_config

```
PermitRootLogin no
```
```
sudo systemctl restart sshd
```


Faveo Installation
------------------

### 1. TS-Debian11  (Apache2 with MySQL)
```
Domain: https://debian.thetamilselvan.in
Username: admin
password: Faveo@123
```

### 2. TS-Rocky9  (Nginx with MariaDB)

```
Domain: https://rocky.thetamilselvan.in
Username: admin
password: Faveo@123
```

Web Server Hardening
--------------------


### 1. Apache Hardening

#### 1. Remove Server Version Banner
```
nano /etc/apache2/conf-enabled/security.conf 
```

```
ServerTokens Prod
ServerSignature Off
```
systemctl restart apache2




#### 2. Nginx

#### 1. Remove Server Version Banner
```
nano /etc/nginx/nginx.conf
```

```
server_tokens off;
```
systemctl restart apache2
DB Optimization
---------------

1. Mysql


2. MariaDB



Webmin
------


### 1. TS-Debian11 
```
Domain: https://debian.thetamilselvan.in:10000
Username: root
password: Ladybird@123$#
```
```
CSF Enabled:  22,80,443,10000,10050,10051,30001,30002,30133
```
### 2. TS-Rocky9

```
   Domain: https://rocky.thetamilselvan.in:10000
   Username: root
   password: Ladybird@123$#
```
```
   CSF Enabled:  22,80,443,10000,10050,10051,30001,30002,30133
```

Zabbix Installation
-------------------

### 1. Zabbix Server

```
   Domain: https://monitoring.thetamilselvan.in/
   Username: admin
   password: Ladybird@123$#
```

### 2. Host
```
   1. TS-Debian11
   2. TS-Rocky9
``` 

### 3. Add Virtual Host

```
nano /etc/zabbix/apache.conf
```

```
<VirtualHost *:80>     
   DocumentRoot /usr/share/zabbix     
   ServerName monitoring.thetamilselvan.in 
</VirtualHost>
```
```
systemctl restart zabbix-server zabbix-agent apache2
```

### 4. Setting up the SSL certificate

```
certbot --apache -d monitoring.thetamilselvan.in
```

### 5. Zabbix Server Configration

To read the Zabbix Server logs, on the Zabbix Server type,

```
tail -f /var/log/zabbix/zabbix_server.log
```

### 6. Zabbix Agent Configration

```
nano /etc/zabbix/zabbix_agentd.conf
```
```
Server=127.0.0.1
ServerActive=127.0.0.1
Hostname=TS-Rocky9
```
```
systemctl restart zabbix-agent
```

To read the Zabbix Agent log files on the host

```
tail -f /var/log/zabbix/zabbix_agentd.log
```

### 7. Host

#### Templates

1. Zabbix server health
2. apache by HTTP
3. Nginx by HTTP
4. Linux CPU by Zabbix agent
5. Linux memory by Zabbix agent
6. Linux network interfaces by Zabbix agent
7. MySQL by Zabbix agent


Ansible AWX
-----------

### 1. Deploy Ansible AWX into a k3s single node cluster

```
sudo apt update && sudo apt -y upgrade
```

#### First set up k3s

```
curl -sfL https://get.k3s.io | sh -s - --write-kubeconfig-mode 644
```

The next step is to validate our installation of K3s using kubectl command which was installed and configured by installer script.

```
kubectl get nodes
NAME             STATUS   ROLES                 AGE  VERSION
ip-10–xx–xx–xx   Ready    control-plane,master  36d  v1.21.5+k3s2{}
```
### Deploy AWX Operator on Kubernetes

Install git and make tools:

```
sudo apt update
sudo apt install git build-essential
```

#### Install AWX Operator

After cloning this repository, you must choose the tag to run:

```
git clone https://github.com/ansible/awx-operator.git
cd awx-operator
git checkout tags/2.12.0
```

If you work from a fork and made modifications since the tag was issued, you must provide the VERSION number to deploy. Otherwise the operator will get stuck in "ImagePullBackOff" state:

```
export VERSION=2.12.0
```

I’ll name mine awx:

```
export NAMESPACE=awx
kubectl create ns ${NAMESPACE}
```

Set current context to value set in NAMESPACE variable:

```
kubectl config set-context --current --namespace=$NAMESPACE 
Context "default" modified.
```

There is a make target you can run:

```
make deploy
```

Otherwise, you can manually create a file called kustomization.yaml with the following content:

```
nano kustomization.yaml
```

```yml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:
  # Find the latest tag here: https://github.com/ansible/awx-operator/releases
  - github.com/ansible/awx-operator/config/default?ref=2.12.0

# Set the image tags to match the git version from above
images:
  - name: quay.io/ansible/awx-operator
    newTag: 2.12.0

# Specify a custom namespace in which to install AWX
namespace: awx
```
```
kubectl apply -k .
```

output:
```
namespace/awx created
customresourcedefinition.apiextensions.k8s.io/awxbackups.awx.ansible.com created
customresourcedefinition.apiextensions.k8s.io/awxrestores.awx.ansible.com created
customresourcedefinition.apiextensions.k8s.io/awxs.awx.ansible.com created
serviceaccount/awx-operator-controller-manager created
role.rbac.authorization.k8s.io/awx-operator-awx-manager-role created
role.rbac.authorization.k8s.io/awx-operator-leader-election-role created
clusterrole.rbac.authorization.k8s.io/awx-operator-metrics-reader created
clusterrole.rbac.authorization.k8s.io/awx-operator-proxy-role created
rolebinding.rbac.authorization.k8s.io/awx-operator-awx-manager-rolebinding created
rolebinding.rbac.authorization.k8s.io/awx-operator-leader-election-rolebinding created
clusterrolebinding.rbac.authorization.k8s.io/awx-operator-proxy-rolebinding created
configmap/awx-operator-awx-manager-config created
service/awx-operator-controller-manager-metrics-service created
deployment.apps/awx-operator-controller-manager created
```

Wait a bit and you should have the awx-operator running:

```
kubectl get pods -n awx
NAME                                               READY   STATUS    RESTARTS   AGE
awx-operator-controller-manager-66ccd8f997-rhd4z   2/2     Running   0          11s
```

Next, create a file named awx-demo.yml in the same folder with the suggested content below. The metadata.name you provide will be the name of the resulting AWX deployment.

Note: If you deploy more than one AWX instance to the same namespace, be sure to use unique names

```
nano awx.demo.yml
```

```yml
---
apiVersion: awx.ansible.com/v1beta1
kind: AWX
metadata:
  name: awx-demo
spec:
  service_type: nodeport
  nodeport_port: 30080
```

Make sure to add this new file to the list of "resources" in your kustomization.yaml file:

```yml
resources:
  - github.com/ansible/awx-operator/config/default?ref=<tag>
  # Add this extra line:
  - awx-demo.yml
```

Finally, apply the changes to create the AWX instance in your cluster:

```
kubectl apply -k .
```

After a few minutes, the new AWX instance will be deployed. You can look at the operator pod logs in order to know where the installation process is at:

```
kubectl logs -f deployments/awx-operator-controller-manager -c awx-manager
```

After a few seconds, you should see the operator begin to create new resources:

```
kubectl get pods -l "app.kubernetes.io/managed-by=awx-operator"
NAME                        READY   STATUS    RESTARTS   AGE
awx-demo-77d96f88d5-pnhr8   4/4     Running   0          3m24s
awx-demo-postgres-0         1/1     Running   0          3m34s

kubectl get svc -l "app.kubernetes.io/managed-by=awx-operator"
NAME                TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
awx-demo-postgres   ClusterIP   None           <none>        5432/TCP       4m4s
awx-demo-service    NodePort    10.109.40.38   <none>        80:31006/TCP   3m56s
```

By default, the admin user is admin and the password is available in the resourcename-admin-password</code> secret. To retrieve the admin password, run:

```
kubectl get secret awx-demo-admin-password -o jsonpath="{.data.password}" | base64 --decode ; echo
yDL2Cx5Za94g9MvBP6B73nzVLlmfgPjR
```


### Stop K3S:

```
/usr/local/bin/k3s-killall.sh
```

### Start K3S: 
```
systemctl start k3s
```


### 2. Ansible Server

```
Domain: http://ansible.thetamilselvan.in:30133
Username: admin
password: Ladybird@123$#
```

### 3. Host 
1. TS-Debian12

#### Host Variables

```yaml
---
ansible_host: 65.108.243.176
```

2. TS-Rocky9

```yaml
---
ansible_host: 212.2.246.166
```


### 4. Ansible Playbook

```yml
- hosts: all
  become: true
  become_user: root
  tasks:
    - name: find all files that are older than 1 day in /var/log
      find:
        paths: /var/log
        age: 1d
        recurse: yes
      register: filesOlderThan1_var_log

    - name: find all files that are older than 1 day in /var/www/faveo/storage/logs
      find:
        paths: /var/www/faveo/storage/logs
        age: 1d
        recurse: yes
      register: filesOlderThan1_faveo_logs

    - name: remove older than 1 day in /var/log
      file:
        path: "{{ item.path }}" 
        state: absent
      with_items: "{{ filesOlderThan1_var_log.files }}"

    - name: remove older than 1 day in /var/www/faveo/storage/logs
      file:
        path: "{{ item.path }}" 
        state: absent
      with_items: "{{ filesOlderThan1_faveo_logs.files }}"
```



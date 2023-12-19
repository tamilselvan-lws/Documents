# Harbor Installation and Configuration

### Prerequisites :

* Kubernetes cluster 1.20+
* Helm v3.2.0+
* A fully registered domain name with an available A record. You can use the domain registrar of your choice. Don’t worry about associating your domain’s A record with an IP at this time. Once you deploy Nginx ingress controller and your Load balancer is attached to your cluster, you will connect your_domain to the proper IP.

## Configuring Nginx Ingress Controller

The Nginx Ingress Controller consists of a Pod and a Service. The Pod runs the Controller, which constantly polls the /ingresses endpoint on the API server of your cluster for updates to available Ingress Resources. The Service is of type LoadBalancer, the cluster will automatically create a Load Balancer on your cloud provider, through which all external traffic will flow to the Controller. The Controller will then route the traffic to appropriate Services, as defined in Ingress Resources.

Only the LoadBalancer Service knows the IP address of the automatically created Load Balancer. To install the Nginx Ingress Controller to your cluster, you’ll first need to add its repository to Helm by running:
```sh
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
```
Update your local Helm chart repository cache:
```sh
helm repo update
```
Create a namespace for Ingress Nginx:
```sh
kubectl create ns ingress-nginx
```
Finally, run the following command to install the Nginx ingress:

```sh
helm install ingress-nginx ingress-nginx/ingress-nginx --namespace=ingress-nginx
```
The above command installs the Nginx Ingress Controller from the stable charts repository and deploys a LoadBalancer. You can watch the LoadBalancer become availble by running. 
```sh
kubectl --namespace ingress-nginx get services -o wide -w ingress-nginx-controller
```
At this point once the External IP is availble copy that and set the A record for your domain in your registrar.

You’ve installed the Nginx Ingress maintained by the Kubernetes community. It will route HTTP and HTTPS traffic from the Load Balancer to appropriate back-end Services, configured in Ingress Resources. In the next step, you’ll expose the Faveo Helpdesk app deployments using an Ingress Resource.

### Securing the Ingress Using Cert-Manager

To secure your Ingress Resources, you’ll install Cert-Manager, create a ClusterIssuer for production, and modify the configuration of your Ingress to take advantage of the TLS certificates. ClusterIssuers are Cert-Manager Resources in Kubernetes that provision TLS certificates for the whole cluster. Once installed and configured, your app will be running behind HTTPS.

Add the Jetstack Helm repository:
```sh
helm repo add jetstack https://charts.jetstack.io
```
Update your local Helm chart repository cache:
```sh
helm repo update
```
Install cert-manager and its Custom Resource Definitions (CRDs) like Issuers and ClusterIssuers
```sh
helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.8.0 \
  --set installCRDs=true
```
To verify our installation, check the cert-manager Namespace for running pods:
```sh
kubectl get pods --namespace cert-manager
```
You’ll now create one that issues Let’s Encrypt certificates, and you’ll store its configuration in a file named **cluster_issuer.yaml**. Create it and open it for editing.

Add the following lines:

```yml
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt-prod
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: your-email@example.com
    privateKeySecretRef:
      name: letsencrypt-prod
    solvers:
      - http01:
          ingress:
            class: nginx
```

This configuration defines a ClusterIssuer that contacts Let’s Encrypt in order to issue certificates. You’ll need to replace your_email_address with your email address in order to receive possible urgent notices regarding the security and expiration of your certificates.

Save and close the file.

Roll it out with kubectl:

```sh
kubectl apply -f cluster_issuer.yml
```

### Install Harbor with Helm

Create a **values.yaml** file for Harbor:


This configuration appears to be a YAML file for deploying the Harbor container registry with Kubernetes. Below is a brief summary and some notes for the expose:

- <b>Type:</b> Ingress
- <b>Controller:</b> Nginx
- <b>Ingress Settings:</b>
  - Host: <b>'your_domain_name'</b>
  - Class Name: <b>'nginx'</b>
  - Annotations:
    - <b>cert-manager.io/cluster-issuer:</b> "letsencrypt-prod"
    - <b>kubernetes.io/ingress.class:</b> "nginx"
    - <b>nginx.ingress.kubernetes.io/ssl-redirect:</b> "true"
    - <b>nginx.ingress.kubernetes.io/proxy-body-size:</b> "0"
    - <b>nginx.ingress.kubernetes.io/rewrite-target:</b> /
  - TLS Settings:
    - Secret Name: <b>'harbor-tls'</b>
    - Hosts: <b>'your_domain_name'</b>
  - <b>Harbor Admin Password:</b> "Harbor12345"
  - <b>External URL:</b> https://your_domain_name
  - <b>Storage Settings:</b>
    - Persistent Volume Claim for Registry, Jobservice, Database, Redis, and Trivy.
  - <b>Image Versions:</b>
    - Nginx: <b>'goharbor/nginx-photon:v2.9.1'</b>
    - Portal: <b>'goharbor/harbor-portal:v2.9.1'</b>
    - Core: <b>'goharbor/harbor-core:v2.9.1'</b>
    - Jobservice: <b>'goharbor/harbor-jobservice:v2.9.1'</b>
    - Registry: <b>'goharbor/registry-photon:v2.9.1'</b>
    - Trivy: <b>'goharbor/trivy-adapter-photon:v2.9.1'</b>
    - Database: <b>'goharbor/harbor-db:v2.9.1'</b> (Internal type)
    - Redis: <b>'goharbor/redis-photon:v2.9.1'</b> (Internal type)
    - Exporter: <b>'goharbor/harbor-exporter:v2.9.1'</b>

Notes:
- Make sure to replace **your_domain_name** with your actual domain name.
- The Harbor Admin Password is set to **"Harbor12345"** - consider changing it for security.
- Storage settings are currently left blank. You might want to specify storage classes as needed.
- All components are configured to use version 2.9.1 of the Harbor images.

Ensure that you have the necessary prerequisites (like a running Kubernetes cluster, required storage classes, etc.) before applying this configuration. Regularly check for updates to Harbor and its dependencies for security and feature improvements.

```yml
expose:
  type: ingress
  controller: true
  ingress:
    hosts:
      core: your_domain_name
    className: "nginx"
    annotations:
      cert-manager.io/cluster-issuer: "letsencrypt-prod"
      kubernetes.io/ingress.class: "nginx"
      nginx.ingress.kubernetes.io/ssl-redirect: "true"
      nginx.ingress.kubernetes.io/proxy-body-size: "0"
      nginx.ingress.kubernetes.io/rewrite-target: /
    tls:
      - secretName: harbor-tls
        hosts:
          - your_domain_name

harborAdminPassword: "Harbor12345"

externalURL: https://your_domain_name

# Storage settings
persistence:
  persistentVolumeClaim:
    registry:
      storageClass: ""
    jobservice:
      jobLog:
        storageClass: ""
    database:
      storageClass: ""
    redis:
      storageClass: ""
    trivy:
      storageClass: ""

# tag 
nginx:
  image:
    repository: goharbor/nginx-photon
    tag: v2.9.1

portal:
  image:
    repository: goharbor/harbor-portal
    tag: v2.9.1

core:
  image:
    repository: goharbor/harbor-core
    tag: v2.9.1

jobservice:
  image:
    repository: goharbor/harbor-jobservice
    tag: v2.9.1

registry:
  image:
    repository: goharbor/registry-photon
    tag: v2.9.1

trivy:
  image:
    repository: goharbor/trivy-adapter-photon
    tag: v2.9.1

database:
    image:
      repository: goharbor/harbor-db
      tag: v2.9.1

redis:
    image:
      repository: goharbor/redis-photon
      tag: v2.9.1

exporter:
  image:
    repository: goharbor/harbor-exporter
    tag: v2.9.1
```

### Download Harbor

Download Harbor helm chart:

```sh
helm repo add harbor https://helm.goharbor.io
helm repo update
```

Create namespace
```sh
kubectl create namespace harbor
```

Install the Harbor helm chart:
```sh
helm install harbor harbor/harbor \
  --namespace harbor \
  -f values.yaml
```

Verify SSL Configuration:
```sh
kubectl get certificates -n harbor
```
Ensure that the certificate is in the "Ready" state.

Now, your Harbor deployment should be accessible over HTTPS with SSL configured. Make sure to adjust the configurations according to your specific setup and requirements.


### Upgrade Images Version: 
To change the version of the Harbor images to v2.10.0-dev, you will need to update the image tags in your Helm chart values.yaml file. Here's an example of how you can modify the values.yaml file:

```yml
expose:
  type: ingress
  controller: true
  ingress:
    hosts:
      core: your_domain_name
    className: "nginx"
    annotations:
      cert-manager.io/cluster-issuer: "letsencrypt-prod"
      kubernetes.io/ingress.class: "nginx"
      nginx.ingress.kubernetes.io/ssl-redirect: "true"
      nginx.ingress.kubernetes.io/proxy-body-size: "0"
      nginx.ingress.kubernetes.io/rewrite-target: /
    tls:
      - secretName: harbor-tls
        hosts:
          - your_domain_name

harborAdminPassword: "Harbor12345"

externalURL: https://your_domain_name

# Storage settings
persistence:
  persistentVolumeClaim:
    registry:
      storageClass: ""
    jobservice:
      jobLog:
        storageClass: ""
    database:
      storageClass: ""
    redis:
      storageClass: ""
    trivy:
      storageClass: ""

# tag
nginx:
  image:
    repository: goharbor/nginx-photon
    tag: v2.10.0-dev

portal:
  image:
    repository: goharbor/harbor-portal
    tag: v2.10.0-dev

core:
  image:
    repository: goharbor/harbor-core
    tag: v2.10.0-dev

jobservice:
  image:
    repository: goharbor/harbor-jobservice
    tag: v2.10.0-dev

registry:
  image:
    repository: goharbor/registry-photon
    tag: v2.10.0-dev

trivy:
  image:
    repository: goharbor/trivy-adapter-photon
    tag: v2.10.0-dev

database:
    image:
      repository: goharbor/harbor-db
      tag: v2.10.0-dev

redis:
    image:
      repository: goharbor/redis-photon
      tag: v2.10.0-dev

exporter:
  image:
    repository: goharbor/harbor-exporter
    tag: v2.10.0-dev
```

Replace the existing image tags with v2.10.0. After making these changes, you can upgrade your Harbor installation using Helm:

```
helm upgrade --install harbor harbor/harbor --namespace harbor -f values.yaml
```

This will apply the new image versions to your Harbor deployment. Ensure that the values.yaml file is correctly configured and adapted to your specific setup before performing the upgrade.
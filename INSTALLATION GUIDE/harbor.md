# Installation with Helm

### Configuring Nginx Ingress Controller
```sh
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
kubectl create ns ingress-nginx
helm install ingress-nginx ingress-nginx/ingress-nginx --namespace=ingress-nginx
```
```sh
kubectl --namespace ingress-nginx get services -o wide -w ingress-nginx-controller
```
```sh
kubectl create ns harbor
kubens harbor
```

#### ingress.yml
```yml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
    annotations:
      kubernetes.io/ingress.class: nginx
    name: harbor-ingress
    namespace: harbor

spec:
   rules:
    - host: "your_domain_name"
      http:
        paths:
         - path: /
           pathType: Prefix
           backend:
             service:
               name: kubernetes
               port:
                 number: 80
```
```sh
kubectl apply -f ingress.yml
```
### Cert-Manager
```sh
helm repo add jetstack https://charts.jetstack.io
helm repo update
```
```sh
helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.8.0 \
  --set installCRDs=true
```
```sh
kubectl get pods --namespace cert-manager
```

#### issuer.yml

```yml
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt-prod
spec:
  acme:
    # Email address used for ACME registration
    email: your_email_address
    server: https://acme-v02.api.letsencrypt.org/directory
    privateKeySecretRef:
      # Name of a secret used to store the ACME account private key
      name: letsencrypt-prod
    # Add a single challenge solver, HTTP01 using nginx
    solvers:
    - http01:
        ingress:
          class: nginx
```
```
kubectl apply -f issuer.yml
```

#### ingress.yml
```yml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
    annotations:
      kubernetes.io/ingress.class: nginx
      cert-manager.io/cluster-issuer: letsencrypt-prod
    name: harbor-ingress
    namespace: harbor

spec:
   tls:
   - hosts:
     - "your_domain_name"
     secretName: harbor-tls
   rules:
    - host: "your_domain_name"
      http:
        paths:
         - path: /
           pathType: Prefix
           backend:
             service:
               name: kubernetes
               port:
                 number: 80
```
```sh
kubectl apply -f ingress.yml
```
```sh
kubectl get certificate
```

### Add repository and fetch harbor helm chart
```sh
helm repo add harbor https://helm.goharbor.io
helm fetch harbor/harbor --untar
```

### Set custom configuration in the values file
```sh
nano harbor/values.yaml
```

Add Core:
```yml
core: "-- Domain-name --"
```

Add <b>ClassName: nginx</b> and add this Values<b>nginx.org/client-max-body-size: "0"</b>
```yml
kubeVersionOverride: ""
    className: "nginx"
    annotations:
      # note different ingress controllers may require a different ssl-redirect annotation
      # for Envoy, use ingress.kubernetes.io/force-ssl-redirect: "true" and remove the nginx lines below
      ingress.kubernetes.io/ssl-redirect: "true"
      ingress.kubernetes.io/proxy-body-size: "0"
      nginx.ingress.kubernetes.io/ssl-redirect: "true"
      nginx.ingress.kubernetes.io/proxy-body-size: "0"
      nginx.org/client-max-body-size: "0"
```

```yml
externalURL: "-- Domain-name --"
```
```yml
harborAdminPassword: "Harbor12345"
```

get storage class value.

```sh
kubectl get storageclass
```
replace all the storage class value in harbor/values.yaml.

```yml
storageClass:
```
### Install

```sh
helm install harbor harbor
```
```sh
kubectl get certificate
```

### Verify installation

```sh
kubectl get pods
kubectl get pvc
kubectl get svc
kubectl get ingress
```


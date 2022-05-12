# Task 4
### Check what I can do
```bash
kubectl auth can-i create deployments --namespace kube-system
```
![](pics/task-4-cani.png)

### Configure user authentication using x509 certificates
### Create private key
```bash
openssl genrsa -out k8s_user.key 2048
```
### Create a certificate signing request
```bash
openssl req -new -key k8s_user.key \
-out k8s_user.csr \
-subj "/CN=k8s_user"
```
### Sign the CSR in the Kubernetes CA. We have to use the CA certificate and the key, which are usually in /etc/kubernetes/pki. But since we use minikube, the certificates will be on the host machine in ~/.minikube
```bash
openssl x509 -req -in k8s_user.csr \
-CA ~/.minikube/ca.crt \
-CAkey ~/.minikube/ca.key \
-CAcreateserial \
-out k8s_user.crt -days 500
```
![](pics/task-4-cert.png)
### Create user in kubernetes
```bash
kubectl config set-credentials k8s_user \
--client-certificate=k8s_user.crt \
--client-key=k8s_user.key
```
![](pics/task-4-user.png)
### Set context for user
```bash
kubectl config set-context k8s_user \
--cluster=minikube --user=k8s_user
```
![](pics/task-4-user-2.png)

### Switch to use new context
```bash
kubectl config use-context k8s_user
```
![](pics/task-4-context.png)
### Check privileges
```bash
kubectl get node
kubectl get pod
```
![](pics/task-4-privilege.png)
### Switch to default(admin) context
```bash
kubectl config use-context minikube
```
![](pics/task-4-minikube.png)
### Bind role and clusterrole to the user
```bash
kubectl apply -f binding.yaml
```
![](pics/task-4-binding.png)
### Check output
```bash
kubectl get pod
```
![](pics/task-4-getpod.png)


### Homework
* Create users deploy_view and deploy_edit. Give the user deploy_view rights only to view deployments, pods. Give the user deploy_edit full rights to the objects deployments, pods.

Generate key, cert and create deploy-edit user in K8s

![](pics/homework-4-deployedit.png)

apply deploy-edit role

![](pics/homework-4-deployedit-2.png)

switch to user context

![](pics/homework-4-useredit.png)

and check permissions

![](pics/homework-4-editpermission.png)

Generate key, cert and create deploy-view user in K8s

![](pics/homework-4-deployview.png)

apply deploy-view role

![](pics/homework-4-deployview-2.png)

switch to user context

![](pics/homework-4-userview.png)

and check permissions

![](pics/homework-4-viewpermission.png)

* Create namespace prod. Create users prod_admin, prod_view. Give the user prod_admin admin rights on ns prod, give the user prod_view only view rights on namespace prod.

Namespace

![](pics/homework-4-createns.png)

Generate key, cert and create prod-admin user in k8s

![](pics/homework-4-prodadmin.png)

Apply role

![](pics/homework-4-prodadmin-2.png)

switch to user context

![](pics/homework-4-prodadmin-3.png)

Check permissions with access-matrix cluster scope

![](pics/homework-4-prodadmin-4.png)

Check permissions with access-matrix in prod namespace

![](pics/homework-4-prodadmin-5.png)

Generate key, cert and create prod-view user in k8s

![](pics/homework-4-prodview.png)

Apply role

![](pics/homework-4-prodview-2.png)

switch to user context

![](pics/homework-4-prodview-3.png)

Check permissions with access-matrix cluster scope

![](pics/homework-4-prodview-4.png)

Check permissions with access-matrix in prod namespace

![](pics/homework-4-prodview-5.png)

* Create a serviceAccount sa-namespace-admin. Grant full rights to namespace default. Create context, authorize using the created sa, check accesses.

Apply role

![](pics/homework-4-sa-namespace-admin-1.png)

Check permissions with access-matrix cluster scope

![](pics/homework-4-sa-namespace-admin-2.png)

Check permissions with access-matrix in default namespace
```bash
kubectl access-matrix --as=system:serviceaccount:default:sa-namespace-admin -n default
```
![](pics/homework-4-sa-namespace-admin-3.png)

Check what is visible in kubernetes-dashboard:

get a token
```bash
kubectl -n default describe secret $(kubectl -n default get secret | grep sa-namespace-admin | awk '{print $1}')
```
![](pics/homework-4-sa-namespace-admin-4.png)

authorize in dashboard with it and check what is visible

![](pics/homework-4-sa-namespace-admin-5.png)

but can see and edit everything in default namespace

![](pics/homework-4-sa-namespace-admin-6.png)
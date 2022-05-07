# Task 3
### [Read more about CSI](https://habr.com/ru/company/flant/blog/424211/)
### Create pv in kubernetes
```bash
kubectl apply -f pv.yaml
```
![pv](pics/task-3-pv.png)
### Check our pv
```bash
kubectl get pv
```
![pv](pics/task-3-pvcheck.png)

### Create pvc
```bash
kubectl apply -f pvc.yaml
```
![pvc](pics/task-3-pvc.png)
### Check our output in pv 
```bash
kubectl get pv
```
![pvc](pics/task-3-pvccheck.png)

### Check pvc
```bash
kubectl get pvc
```
![pvc](pics/task-3-pvccheck2.png)
### Apply deployment minio
```bash
kubectl apply -f deployment.yaml
```
![deploy](pics/task-3-deploy.png)
### Apply svc nodeport
```bash
kubectl apply -f minio-nodeport.yaml
```
![deploy](pics/task-3-svc.png)

Open minikup_ip:node_port in you browser

![deploy](pics/task-3-nodeport.png)
![deploy](pics/task-3-monio.png)
### Apply statefulset
```bash
kubectl apply -f statefulset.yaml
```
![statefullset](pics/task-3-statefullset.png)
### Check pod and statefulset
```bash
kubectl get pod
kubectl get sts
```
![statefullset](pics/task-3-stscheck.png)
### Homework
* We published minio "outside" using nodePort. Do the same but using ingress.

First i changed port for WEB-UI in statefullset to be 8080

![homework-1](pics/homework-3-statefullset.png)

applied it with ingress manifest
```bash
kubectl apply -f homework/
```
![homework-1](pics/homework-3-ingress-1.png)

and checked avalability

![homework-1](pics/homework-3-webui.png)
* Publish minio via ingress so that minio by ip_minikube and nginx returning hostname (previous job) by path ip_minikube/web are available at the same time.

I changed ingress configuration and applied it

![homework-2](pics/homework-3-ingress-2.png)

WEB-UI is still available

![homework-2](pics/homework-3-webui-2.png)

/web path returs hostname

![homework-2](pics/homework-3-webpath.png)
* Create deploy with emptyDir save data to mountPoint emptyDir, delete pods, check data.

Apply deploy

![homework-3](pics/homework-3-deploy.png)

Upload testfile

![homework-3](pics/homework-3-testfile.png)

![homework-3](pics/homework-3-test-bucket.png)

delete pod and new one was created

![homework-3](pics/homework-3-delete.png)

Bucket lost

![homework-3](pics/homework-3-buckets.png)

* Optional. Raise an nfs share on a remote machine. Create a pv using this share, create a pvc for it, create a deployment. Save data to the share, delete the deployment, delete the pv/pvc, check that the data is safe.

After export of NFS share I mounted it inside of Minikube

![homework-4](pics/homework-3-nfs.png)

then I applied pv.yaml

![homework-4](pics/homework-3-pv.png)

and pvc.yaml and checked that it is bound

![homework-4](pics/homework-3-pvc.png)

applied deployment-nfs-share.yaml

![homework-4](pics/homework-3-nfsdeploy.png)

uploaded testfile to bucket again

![homework-4](pics/homework-3-nfsbucket.png)

and after deletition of deployment checked my files

![homework-4](pics/homework-3-check.png)
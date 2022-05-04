# Task 2
### ConfigMap & Secrets
![ConfigMap](pics/task-2-configmap.png)
### Check env in pod
![printenv](pics/task-2-printenv.png)
### Create deployment with simple application
![deploy](pics/task-2-deploy.png)
### Get pod ip address
![podips](pics/task-2-podips.png)
* Try connect to pod with curl (curl pod_ip_address). What happens?
* From your PC

![curlpc](pics/task-2-curlpc.png)
* From minikube (minikube ssh)

![curlssh](pics/task-2-curlssh.png)
* From another pod (kubectl exec -it $(kubectl get pod |awk '{print $1}'|grep web-|head -n1) bash)

![curlexec](pics/task-2-curlexec.png)
### Create service (ClusterIP)
Get service CLUSTER-IP

![service](pics/task-2-service.png)
* Try connect to service (curl service_ip_address). What happens?

* From you PC

![curlpc-2](pics/task-2-curlpc-2.png)
* From minikube (minikube ssh) (run the command several times)

![curlssh-2](pics/task-2-curlssh-2.png)
* From another pod (kubectl exec -it $(kubectl get pod |awk '{print $1}'|grep web-|head -n1) bash) (run the command several times)

![curlexec-2](pics/task-2-curlexec-2.png)
### NodePort
![nodeport](pics/task-2-nodeport.png)
### Checking the availability of the NodePort service type
![checkport](pics/task-2-checkport.png)
### Headless service
![headless](pics/task-2-headless.png)
### DNS
Connect to any pod

![resolv](pics/task-2-resolv.png)

Compare the IP address of the DNS server in the pod and the DNS service of the Kubernetes cluster.

![resolvssh](pics/task-2-resolvssh.png)
* Compare headless and clusterip
Inside the pod run nslookup to normal clusterip and headless. Compare the results.
You will need to create pod with dnsutils.

![nslookup](pics/task-2-nslookup.png)

### [Ingress](https://kubernetes.github.io/ingress-nginx/deploy/#minikube)
Enable Ingress controller

![ingress](pics/task-2-ingress-1.png)

Let's see what the ingress controller creates for us

![ingress](pics/task-2-ingress-2.png)

![ingress](pics/task-2-ingress-3.png)

Create Ingress

![ingress](pics/task-2-ingress-4.png)

### Homework
* In Minikube in namespace kube-system, there are many different pods running. Your task is to figure out who creates them, and who makes sure they are running (restores them after deletion).
![kube-system](pics/homework-2-kube-system.png)

I used
```bash 
kubectl describe pod -n kube-system kube-apiserver-minikube
```
and found out that it is controlled by
```bash 
Controlled By:  Node/minikube
```
same thing about kube-scheduler-minikube it is "Controlled By:  Node/minikube"

* Implement Canary deployment of an application via Ingress. Traffic to canary deployment should be redirected if you add "canary:always" in the header, otherwise it should go to regular deployment.
Set to redirect a percentage of traffic to canary deployment.

Applied two namespaces prod and canary

![namespaces](pics/homework-2-namespaces.png)

configmaps for nginx in corresponding namespaces

![configmaps](pics/homework-2-configmaps.png)

services as well

![services](pics/homework-2-services.png)

and applied deployments

![configmaps](pics/homework-2-deployments.png)

lastly applied ingress for prod and canary

![ingress](pics/homework-2-ingress.png)

and checked that it is working with and without canary: always

![curl](pics/homework-2-curl.png)
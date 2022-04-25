# Task 1.1
## Verify kubectl installation
```bash
kubectl version --client
```
![version](pics/task-1.1-kubectl.png)
## Get information about cluster
```bash
kubectl cluster-info
```
![cluster-info](pics/task-1.1-cluster-info.png)
## get information about available nodes
```bash
kubectl get nodes
```
![nodes](pics/task-1.1-nodes.png)
# Install [Kubernetes Dashboard](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/)
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.5.1/aio/deploy/recommended.yaml
```
![dashboard-apply](pics/task-1.1-dashboard-apply.png)
# Install [Metrics Server](https://github.com/kubernetes-sigs/metrics-server#deployment)
```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```
![metrics](pics/task-1.1-metrics.png)
# Connect to Dashboard
![dashboard-web-ui](./pics/task-1.1-dashboard.png)
# Task 1.2
# Kubernetes resources introduction
```bash
kubectl run web --image=nginx:latest
```
- take a look at created resource in cmd "kubectl get pods"

![nginx-cmd](pics/task-1.2-nginx-kubectl.png)
- take a look at created resource in Dashboard

![nginx-dashboard](pics/task-1.2-nginx-dashboard.png)
- take a look at created resource in cmd

![nginx-ssh](pics/task-1.2-nginx-ssh.png)

## Apply manifests (download from repository)
![manifests](pics/task-1.2-manifests.png)

### Homework
* Create a deployment nginx.

![homework-nginx](pics/homework-nginx-deployment.png)
* Set up two replicas.

![homework-replicas](pics/homework-nginx-replicas.png)
* Remove one of the pods, see what happens.

![homework-replicas](pics/homework-nginx-delete.png)

I deleted one pod and k8s created a new one to fulfill replica target
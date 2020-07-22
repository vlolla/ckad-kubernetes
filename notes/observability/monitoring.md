# Monitoring

## Minikube

```
ubuntu@ubuntu-V5-171:~$ minikube addons enable metrics-server
🌟  The 'metrics-server' addon is enabled
ubuntu@ubuntu-V5-171:~$ kubectl top node
error: metrics not available yet
ubuntu@ubuntu-V5-171:~$ kubectl top node
NAME       CPU(cores)   CPU%   MEMORY(bytes)   MEMORY%   
minikube   167m         4%     934Mi           7%        
ubuntu@ubuntu-V5-171:~$ 
```

## Kubernetes Cluster

Let us deploy metrics-server to monitor the PODs and Nodes. Pull the git repository for the deployment files.

https://github.com/kodekloudhub/kubernetes-metrics-server.git

Deploy the metrics-server by creating all the components downloaded.


Run the 'kubectl create -f .' command from within the downloaded repository.


```
master $ git clone https://github.com/kodekloudhub/kubernetes-metrics-server.git
Cloning into 'kubernetes-metrics-server'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 15 (delta 0), reused 0 (delta 0), pack-reused 12
Unpacking objects: 100% (15/15), done.
master $ ls
Desktop  go  kubernetes-metrics-server
master $ cd kubernetes-metrics-server/
master $ ls
aggregated-metrics-reader.yaml  metrics-apiservice.yaml         README.md
auth-delegator.yaml             metrics-server-deployment.yaml  resource-reader.yaml
auth-reader.yaml                metrics-server-service.yaml

master $ kubectl create -f .
clusterrole.rbac.authorization.k8s.io/system:aggregated-metrics-reader created
clusterrolebinding.rbac.authorization.k8s.io/metrics-server:system:auth-delegator created
rolebinding.rbac.authorization.k8s.io/metrics-server-auth-reader created
apiservice.apiregistration.k8s.io/v1beta1.metrics.k8s.io created
serviceaccount/metrics-server created
deployment.apps/metrics-server created
service/metrics-server created
clusterrole.rbac.authorization.k8s.io/system:metrics-server created
clusterrolebinding.rbac.authorization.k8s.io/system:metrics-server created
master $
```
Run the 'kubectl top node' command and wait for a valid output.

```
master $ kubectl top node
NAME     CPU(cores)   CPU%   MEMORY(bytes)   MEMORY%
master   208m         10%    1273Mi          67%
node01   2000m        100%   959Mi           24%
```

## References
• https://kubernetes.io/docs/tasks/debug-application-
cluster/core-metrics-pipeline/
• https://kubernetes.io/docs/tasks/debug-application-
cluster/resource-usage-monitoring/



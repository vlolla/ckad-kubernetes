
Refer URL for more information 
https://kubernetes.io/docs/setup/learning-environment/minikube/


## Start Minikube and create a cluster:

First Time when you create single node cluster on Ubuntu.
```
$ minikube start
```
The output is similar to this:
```
Starting local Kubernetes cluster...
Running pre-create checks...
Creating machine...
Starting local Kubernetes cluster...
```
For more information on starting your cluster on a specific Kubernetes version, VM, or container runtime, see Starting a Cluster.

## Starting a Cluster

The minikube start command can be used to start your cluster. This command creates and configures a Virtual Machine that runs a single-node Kubernetes cluster. This command also configures your kubectl installation to communicate with this cluster.

```
ubuntu@ubuntu-V5-171:~$ minikube start
😄  minikube v1.12.0 on Ubuntu 18.04
✨  Using the docker driver based on existing profile
👍  Starting control plane node minikube in cluster minikube
🔄  Restarting existing docker container for "minikube" ...
🐳  Preparing Kubernetes v1.18.3 on Docker 19.03.2 ...
🔎  Verifying Kubernetes components...
🌟  Enabled addons: dashboard, default-storageclass, storage-provisioner
🏄  Done! kubectl is now configured to use "minikube"
```

## Specifying the Kubernetes version
You can specify the version of Kubernetes for Minikube to use by adding the --kubernetes-version string to the minikube start command. For example, to run version v1.18.0, you would run the following:
```
$ minikube start --kubernetes-version v1.18.0
```
I have done using the docker driver but there are many driver support we get for Minikube refer kuberneter.io documentation for more.

 
## Use local images by re-using the Docker daemon

To work with the Docker daemon on your Mac/Linux host, run the last line from minikube docker-env.

You can now use Docker at the command line of your host Mac/Linux machine to communicate with the Docker daemon inside the Minikube VM:
```
docker ps
```
## Minikube status
```
ubuntu@ubuntu-V5-171:~$ minikube status
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```


## Verify if kubectl is connected to minikube
```
ubuntu@ubuntu-V5-171:~$ kubectl cluster-info
Kubernetes master is running at https://172.17.0.2:8443
KubeDNS is running at https://172.17.0.2:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
```
To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.

## Running minikube dashboard
```
ubuntu@ubuntu-V5-171:~$ minikube dashboard
🤔  Verifying dashboard health ...
🚀  Launching proxy ...
🤔  Verifying proxy health ...
🎉  Opening http://127.0.0.1:34289/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/ in your default browser...
Opening in existing browser session.
[0712/184317.635442:ERROR:nacl_helper_linux.cc(308)] NaCl helper process running without a sandbox!
Most likely you need to configure your SUID sandbox correctly
```
*Ignore sandbox warning for now.


This helps to start the default K8 environment and if you see a browser will open up kubernetes dashboard.
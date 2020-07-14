
# Install and Setup minikube

Install Minikube, a tool that runs a single-node Kubernetes cluster in a virtual machine on your personal computer.

To check if virtualization is supported on Linux, run the following command and verify that the output is non-empty:
```
grep -E --color 'vmx|svm' /proc/cpuinfo
```
Make sure you have kubectl installed

## Install Minikube via direct download

If you're not installing via a package, you can download a stand-alone binary and use that.
```
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 \
  && chmod +x minikube
```
Here's an easy way to add the Minikube executable to your path:
```
sudo mkdir -p /usr/local/bin/
sudo install minikube /usr/local/bin/
```
Install docker on Ubuntu using as we would use docker as driver for minikube. Install instruction on docker on ubuntu; refer https://docs.docker.com/engine/install/ubuntu/#installation-methods

```
buntu@ubuntu-V5-171:~$ sudo docker version
Client:
 Version:           19.03.6
 API version:       1.40
 Go version:        go1.12.17
 Git commit:        369ce74a3c
 Built:             Fri Feb 28 23:45:43 2020
 OS/Arch:           linux/amd64
 Experimental:      false

Server:
 Engine:
  Version:          19.03.6
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.12.17
  Git commit:       369ce74a3c
  Built:            Wed Feb 19 01:06:16 2020
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.3.3-0ubuntu1~18.04.2
  GitCommit:        
 runc:
  Version:          spec: 1.0.1-dev
  GitCommit:        
 docker-init:
  Version:          0.18.0
  GitCommit:        
```
Refer to docker post install to enable docker group for user.
https://docs.docker.com/engine/install/linux-postinstall/

To create the docker group and add your user:

    Create the docker group.
```
    $ sudo groupadd docker
```
    Add your user to the docker group.
```
    $ sudo usermod -aG docker $USER
```
Log out and log back in so that your group membership is re-evaluated.



## Confirm Installation

To confirm successful installation of both a hypervisor and Minikube, you can run the following command to start up  local Kubernetes cluster:

```
minikube start 
```
This would take couple of minutes to download Kubernetes and preload. It automatically detects docker driver and pulls K8 base image and create docker container for Kubernetes environment setup. 

```
ubuntu@ubuntu-V5-171:~/github$ minikube start 
😄  minikube v1.12.0 on Ubuntu 18.04
✨  Automatically selected the docker driver
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
💾  Downloading Kubernetes v1.18.3 preload ...
    > preloaded-images-k8s-v4-v1.18.3-docker-overlay2-amd64.tar.lz4: 526.27 MiB
🔥  Creating docker container (CPUs=2, Memory=2900MB) ...
🐳  Preparing Kubernetes v1.18.3 on Docker 19.03.2 ...
🔎  Verifying Kubernetes components...
🌟  Enabled addons: default-storageclass, storage-provisioner
🏄  Done! kubectl is now configured to use "minikube"
💗  Kubectl not found in your path
👉  You can use kubectl inside minikube. For more information, visit https://minikube.sigs.k8s.io/docs/handbook/kubectl/
💡  For best results, install kubectl: https://kubernetes.io/docs/tasks/tools/install-kubectl/
```

As you see in the above minikube start kubectl is not found, lets install and setup kubectl.

# Install and Setup kubectl

The Kubernetes command-line tool, kubectl, allows you to run commands against Kubernetes clusters. You can use kubectl to deploy applications, inspect and manage cluster resources, and view logs. For a complete list of kubectl operations, 
see Overview of kubectl here https://kubernetes.io/docs/reference/kubectl/overview/
## Install kubectl binary with curl on Linux 

1. Install kubectl binary with curl on Linux 

    ```
    curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
    ```
2. Make the kubectl binary executable.

    ```
    chmod +x ./kubectl
    ```

3. Move the binary in to your PATH.

    ```
    sudo mv ./kubectl /usr/local/bin/kubectl
    ```

4. Test to ensure the version you installed is up-to-date:

    ```
    kubectl version --client
    ```
## Verfiy if kubeclt is working

```
ubuntu@ubuntu-V5-171:~$ kubectl version --client
Client Version: version.Info{Major:"1", Minor:"18", GitVersion:"v1.18.5", GitCommit:"e6503f8d8f769ace2f338794c914a96fc335df0f", GitTreeState:"clean", BuildDate:"2020-06-26T03:47:41Z", GoVersion:"go1.13.9", Compiler:"gc", Platform:"linux/amd64"}

ubuntu@ubuntu-V5-171:~$ kubectl get pods
No resources found in default namespace.
```

## Install using native package management

```
sudo apt-get update && sudo apt-get install -y apt-transport-https gnupg2
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubectl
```
This would install kubectl to test if the kubeclt is installed

If you are using different operation system please visit https://kubernetes.io/docs/tasks/tools/install-kubectl/ for more options. I did using Ubuntu for my version of kubectl


## Verifying kubectl configuration

In order for kubectl to find and access a Kubernetes cluster, it needs a kubeconfig file, which is created automatically when you create a cluster using kube-up.sh or 
successfully deploy a Minikube cluster. By default, kubectl configuration is located at ~/.kube/config. Below are install instructions for Minikube on Ubuntu.

Check that kubectl is properly configured by getting the cluster state: 
```
kubectl cluster-info
```
If you see a URL response, kubectl is correctly configured to access your cluster.

If you see a message similar to the following, kubectl is not configured correctly or is not able to connect to a Kubernetes cluster.

The connection to the server <server-name:port> was refused - did you specify the right host or port?

For example, if you are intending to run a Kubernetes cluster on your laptop (locally), you will need a tool like Minikube to be installed first and then re-run the commands stated above.

If kubectl cluster-info returns the url response but you can't access your cluster, to check whether it is configured properly, use:
```
kubectl cluster-info 
```

```
ubuntu@ubuntu-V5-171:~$ kubectl cluster-info
Kubernetes master is running at https://172.17.0.2:8443
KubeDNS is running at https://172.17.0.2:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.

```

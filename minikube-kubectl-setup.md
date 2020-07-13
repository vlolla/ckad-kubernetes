# Install and Setup kubectl

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
kubectl cluster-info dump
```

# Install and Setup minikube

Install Minikube, a tool that runs a single-node Kubernetes cluster in a virtual machine on your personal computer.

To check if virtualization is supported on Linux, run the following command and verify that the output is non-empty:
```
grep -E --color 'vmx|svm' /proc/cpuinfo
```


## Pod Overview

Pod, the smallest deployable object in the Kubernetes object model.

Please refer for more information https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/

Note: Restarting a container in a Pod should not be confused with restarting a Pod. A Pod is not a process, but an environment for running a container. A Pod persists until it is deleted.

## Pod templates

The sample below is a manifest for a simple Job with a template that starts one container. The container in that Pod prints a message then pauses.

```
apiVersion: batch/v1
kind: Job
metadata:
  name: hello
spec:
  template:
    # This is the pod template
    spec:
      containers:
      - name: hello
        image: busybox
        command: ['sh', '-c', 'echo "Hello, Kubernetes!" && sleep 3600']
      restartPolicy: OnFailure
    # The pod template ends here
```
## Termination of Pods

Because Pods represent running processes on nodes in the cluster, it is important to allow those processes to gracefully terminate when they are no longer needed (vs being violently killed with a KILL signal and having no chance to clean up)

By default, all deletes are graceful within 30 seconds. The kubectl delete command supports the --grace-period=<seconds> option which allows a user to override the default and specify their own value.

## Delete Pods
You can perform a graceful pod deletion with the following command:
```
kubectl delete pods <pod>
```
If you want to delete a Pod forcibly using kubectl version >= 1.5, do the following:
```
kubectl delete pods <pod> --grace-period=0 --force
```
If you're using any version of kubectl <= 1.4, you should omit the --force option and use:
```
kubectl delete pods <pod> --grace-period=0
```
If even after these commands the pod is stuck on Unknown state, use the following command to remove the pod from the cluster:
```
kubectl patch pod <pod> -p '{"metadata":{"finalizers":null}}'
```
Always perform force deletion of StatefulSet Pods carefully and with complete knowledge of the risks involved.


## Updating a Pod Definition

A Note on Editing Existing Pods
In any of the practical quizzes if you are asked to edit an existing POD, please note the following:

* If you are given a pod definition file, edit that file and use it to create a new pod.

* If you are not given a pod definition file, you may extract the definition to a file using the below command:
```
kubectl get pod <pod-name> -o yaml > pod-definition.yaml
```
Then edit the file to make the necessary changes, delete and re-create the pod.

* Use the kubectl edit pod <pod-name> command to edit pod properties.

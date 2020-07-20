# Tains & Tolerations


Node affinity, is a property of Pods that attracts them to a set of nodes (either as a preference or a hard requirement). Taints are the opposite -- they allow a node to repel a set of pods.

Tolerations are applied to pods, and allow (but do not require) the pods to schedule onto nodes with matching taints.

Taints and tolerations work together to ensure that pods are not scheduled onto inappropriate nodes. One or more taints are applied to a node; this marks that the node should not accept any pods that do not tolerate the taints

## Tains to a Node

You add a taint to a node using kubectl taint. 

        kubectl taint nodes node-name key=value:taint-effect

taint-effect possible values: NoSchedule or PreferNoSchedule or NoExecute

For example,

        kubectl taint nodes node1 app=blue:NoSchedule

## Tolerations to a pod

You specify a toleration for a pod in the PodSpec. Both of the following tolerations "match" the taint created by the kubectl taint line above, and thus a pod with either toleration would be able to schedule onto node1:

        tolerations:
        - key: "key"
        operator: "Equal"
        value: "value"
        effect: "NoSchedule"

E.g from Exercises 

kubectl taint nodes node1 spray=mortein:NoSchedule

./examples/configuration/pod-with-toleration.yml
```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    env: test
spec:
  containers:
  - name: nginx
    image: nginx
    imagePullPolicy: IfNotPresent
  tolerations:
  - key: "app"
    operator: "Equal"
    value: "blue"
    effect: "NoSchedule"
```

The default value for operator is Equal.

A toleration "matches" a taint if the keys are the same and the effects are the same, and:

  * the operator is Exists (in which case no value should be specified), or
  * the operator is Equal and the values are equal.

## Taint - NoExecute

The NoExecute taint effect, mentioned above, affects pods that are already running on the node as follows

    * pods that do not tolerate the taint are evicted immediately
    * pods that tolerate the taint without specifying tolerationSeconds in their toleration specification remain bound forever
    * pods that tolerate the taint with a specified tolerationSeconds remain bound for the specified amount of time

## Not to deploy workloads on master

if you want to look at the tains of the master node

        kubectl describe node kubemaster | grep Taint

if you are using minikube

        ubuntu@ubuntu-V5-171:~/github$ kubectl describe node minikube | grep Taint
        Taints:             <none>


Some Commands to know

    
Removes taint on master node

    kubectl taint nodes master node-role.kubernetes.io/master:NoSchedule-

To find which node pod is running.

    kubectl get pods -o wide

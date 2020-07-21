# Node Selectors and Node Affinity

## nodeSelector

nodeSelector is the simplest recommended form of node selection constraint. nodeSelector is a field of PodSpec. It specifies a map of key-value pairs. For the pod to be eligible to run on a node, the node must have each of the indicated key-value pairs as labels (it can have additional labels as well). The most common usage is one key-value pair.

Let's walk through an example of how to use nodeSelector

### Step One: Attach label to the node

Run kubectl get nodes to get the names of your cluster's nodes.

kubectl label nodes <node-name> <label-key>=<label-value>

e.g as i am running this on minikube version

kubectl label nodes minikube disktype=ssd

To view the lables of on the nodes. 

    ubuntu@ubuntu-V5-171:~/github$ kubectl get nodes --show-labels | grep disktype
    minikube   Ready    master   7d4h   v1.18.3   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,disktype=ssd,kubernetes.io/arch=amd64,kubernetes.io/hostname=minikube,kubernetes.io/os=linux,minikube.k8s.io/commit=c83e6c47124b71190e138dbc687d2556d31488d6,minikube.k8s.io/name=minikube,minikube.k8s.io/updated_at=2020_07_13T12_13_08_0700,minikube.k8s.io/version=v1.12.0,node-role.kubernetes.io/master=

### Step Two: Add a nodeSelector field to your pod configuration

 For example, if this is my pod config:

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
  nodeSelector:
    disktype: ssd
```

## Node Affinity

Node affinity is conceptually similar to nodeSelector -- it allows you to constrain which nodes your pod is eligible to be scheduled on, based on labels on the node.

There are currently two types of node affinity, called requiredDuringSchedulingIgnoredDuringExecution and preferredDuringSchedulingIgnoredDuringExecution.

Thus an example of 

requiredDuringSchedulingIgnoredDuringExecution would be "only run the pod on nodes with Intel CPUs" and an example 

preferredDuringSchedulingIgnoredDuringExecution would be "try to run this set of pods in failure zone XYZ, but if it's not possible, then allow some to run elsewhere".

Node affinity is specified as field nodeAffinity of field affinity in the PodSpec.

Here's an example of a pod that uses node affinity:

```
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
spec:
  containers:
    - name: data-processor
      image: data-processor
  
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: size
            operator: In
            values:
            - Large
            - Medium  
```

If you like to put as not in we can have operator as NotIn

```
        - key: size
            operator: NotIn
            values:
            - Small
          
```
```
        - key: size
            operator: Exists
          
```
Not values are checked with operator exists.


What if there is no node as size, what if lable changes in the furture. That can be answered Node Affinity Types

## Node Affinity Types

Available:

        requiredDuringSchedulingIgnoredDuringExecution
        preferredDuringSchedulingIgnoredDuringExecution


Planned:

        requiredDuringSchedulingRequiredDuringExecution

```
            During Scheduling       During Execution

Type  1         Required                Ignored   
Type  2         Preferred               Ignored
Type  3         Required                Required

```
Q. Set Node Affinity to the deployment to place the PODs on node01 only

Name: blue
Replicas: 6
Image: nginx
NodeAffinity: requiredDuringSchedulingIgnoredDuringExecution
Key: color
values: blue

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: blue
spec:
  replicas: 6
  selector:
    matchLabels:
      run: nginx
  template:
    metadata:
      labels:
        run: nginx
    spec:
      containers:
      - image: nginx
        imagePullPolicy: Always
        name: nginx
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: color
                operator: In
                values:
                - blue
```

```

Q7. Create a new deployment named 'red' with the NGINX image and 3 replicas, and ensure it gets placed on the master node only.

Use the label - node-role.kubernetes.io/master - set on the master node.

Name: red
Replicas: 3
Image: nginx
NodeAffinity: requiredDuringSchedulingIgnoredDuringExecution
Key: node-role.kubernetes.io/master
Use the right operator


```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: red
spec:
  replicas: 3
  selector:
    matchLabels:
      run: nginx
  template:
    metadata:
      labels:
        run: nginx
    spec:
      containers:
      - image: nginx
        imagePullPolicy: Always
        name: nginx
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: node-role.kubernetes.io/master
                operator: Exists
```
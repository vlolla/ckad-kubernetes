# Replication Controller

A ReplicationController ensures that a specified number of pod replicas are running at any one time. 
In other words, a ReplicationController makes sure that a pod or a homogeneous set of pods is always up and available.

```
Note: A Deployment that configures a ReplicaSet is now the recommended way to set up replication.
```
Refer official documentation for more informatin on replication controller
https://kubernetes.io/docs/concepts/workloads/controllers/replicationcontroller/

## How a ReplicationController Works
If there are too many pods, the ReplicationController terminates the extra pods. If there are too few, the ReplicationController starts more pods. Unlike manually created pods, the pods maintained by a ReplicationController are automatically replaced if they fail, are deleted, or are terminated. For example, your pods are re-created on a node after disruptive maintenance such as a kernel upgrade. For this reason, you should use a ReplicationController even if your application requires only a single pod. A ReplicationController is similar to a process supervisor, but instead of supervising individual processes on a single node, the ReplicationController supervises multiple pods across multiple nodes.

ReplicationController is often abbreviated to "rc" in discussion, and as a shortcut in kubectl commands.

A simple case is to create one ReplicationController object to reliably run one instance of a Pod indefinitely. A more complex use case is to run several identical replicas of a replicated service, such as web servers.

## Running an example ReplicationController 

./examples/replication.yaml 
```
apiVersion: v1
kind: ReplicationController
metadata:
  name: myapp-rc
  labels:
      app: myapp
      type: front-end
spec:
  metadata:
  name: myapp-pod
  labels:
      app: myapp
  spec:
    containers:
      - name: nginx-container
        image: nginx
 
 replicas: 3
```
Run the example job by downloading the example file under examples and then running this command:

```
kubectl create -f replication-definition.yml
```

Check on the status of the ReplicationController using this command:

```
kubectl get replicationcontroller
```

To know how may pods are running

```
kubectl get pods
```


## Replica Set Vs Replication Controller

Replica Set and Replication Controller do almost the same thing. Both of them ensure that a specified number of pod replicas are running at any given time. 
The difference comes with the usage of selectors to replicate pods. Replica Set use Set-Based selectors while replication controllers use Equity-Based selectors.
Creating Replication Controller


# Replica Set

A ReplicaSet's purpose is to maintain a stable set of replica Pods running at any given time. 
As such, it is often used to guarantee the availability of a specified number of identical Pods.

## How a ReplicaSet works
A ReplicaSet is defined with fields, including a selector that specifies how to identify Pods it can acquire, a number of replicas indicating how many Pods it should be maintaining, and a pod template specifying the data of new Pods it should create to meet the number of replicas criteria. A ReplicaSet then fulfills its purpose by creating and deleting Pods as needed to reach the desired number. When a ReplicaSet needs to create new Pods, it uses its Pod template.

A ReplicaSet is linked to its Pods via the Pods' metadata.ownerReferences field, which specifies what resource the current object is owned by. All Pods acquired by a ReplicaSet have their owning ReplicaSet's identifying information within their ownerReferences field. It's through this link that the ReplicaSet knows of the state of the Pods it is maintaining and plans accordingly.

A ReplicaSet identifies new Pods to acquire by using its selector. If there is a Pod that has no OwnerReference or the OwnerReference is not a Controller and it matches a ReplicaSet's selector, it will be immediately acquired by said ReplicaSet.

## When to use a ReplicaSet
A ReplicaSet ensures that a specified number of pod replicas are running at any given time. However, a Deployment is a higher-level concept that manages ReplicaSets and provides declarative updates to Pods along with a lot of other useful features. Therefore, we recommend using Deployments instead of directly using ReplicaSets, unless you require custom update orchestration or don't require updates at all.

This actually means that you may never need to manipulate ReplicaSet objects: use a Deployment instead, and define your application in the spec section.

## Example

./examples/replicaset-definition.yml

```
apiVersion: apps/v1
kind : ReplicaSet
metadata:
  name: myapp-replicaset
  labels:
      app: myapp
      type: front-end
spec:
    template:
      metadata:
      name: myapp-pod
      labels:
        app: myapp
    spec:
      containers:
        - name: nginx-container
          image: nginx 
  replicas: 3
  selector: 
    matchLabels:
        type: front-end
        
  ```
  
 To create a Replica set run below command
 
 ```
 kubectl create -f replicaset-definition.yml
 ```
 
 Check if the replicaset is created
 
 ```
 kubectl get replicaset
 ```
 
 To get PODS
 
 ```
 kubectl get pods
 ```
 
 For more information on replica set refer URL https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/
 
 ## Labels and Selectors
 
 * Labeling  can help to filter in monitoring the pods with the same name
 * When the replica set monitors and identify that replica count has a  fail; it would  create a new pod using a template defined under specs section.
 
 
  ```
  MAJOR DIFFERENCE IS REPLICA SET REQUIRES SELECTOR --- because RS also can manage pods which are created before replica sets
  ```
  
  ## Scale
  
  *  Use the definition file and run replace command
  
  In the replicaset definition file change replicas to desired number, for e.g take the replicaset-definition.yml change it to 6 and run the below command
  
  ```
  kubectl replace -f replicaset-definition.yml
  ```
  this would update
  
  * Second way to do is run command
  
  ```
  kubectl scale --replicas=6 -f replicaset-definition.yml
  ```
  or
  ```
  kubectl scale --replicas=6 replicaset myapp-replicaset
  ```
  
 Important: Scale command wont update your yml file.
 
 ## Commands We learned in this section
 
 ```
 kubectl create -f replicaset-definition.yml
 
 kubectl get replicaset
 
 kubectl delete replicaset myapp-replicaset *Also deletes all underlying pods in this replicaset
 
 kubectl replace -f replicaset-definition.yml 
 
 kubectl scale --replicas=6 -f replicaset-definition.yml
 
 ```
 
 
 ```
 
 Link to practice tests:
 https://kodekloud.com/courses/kubernetes-certification-course-labs/lectures/12039431

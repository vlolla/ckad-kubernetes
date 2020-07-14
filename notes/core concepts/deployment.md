# Deployments
A Deployment provides declarative updates for Pods and ReplicaSets.

You describe a desired state in a Deployment, and the Deployment Controller changes the actual state to the desired state at a controlled rate. You can define Deployments to create new ReplicaSets, or to remove existing Deployments and adopt all their resources with new Deployments.

## Creating a Deployment 
The following is an example of a Deployment. It creates a ReplicaSet to bring up three nginx Pods:

#
./examples/deployment-definition.yml
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

 Follow the steps given below to create the above Deployment:

1. Create the Deployment by running the following command:

        
        kubectl apply -f deployment-definition.yml
       
    Output
    ```
    deployment.apps/nginx-deployment created
    ```
2. Run kubectl get deployments to check if the Deployment was created.

    ```
    ubuntu@ubuntu-V5-171:~/github/kubernetes/examples$ kubectl get deployments
    NAME               READY   UP-TO-DATE   AVAILABLE   AGE
    nginx-deployment   3/3     3            3           2m1s
    ```

3. To see the Deployment rollout status, run kubectl rollout status deployment.v1.apps/nginx-deployment.

    ```
    ubuntu@ubuntu-V5-171:~/github/kubernetes/examples$ kubectl rollout status deployment.v1.apps/nginx-deployment
    deployment "nginx-deployment" successfully rolled out
    ```
4. Run the kubectl get deployments again a few seconds later. The output is similar to this:

    ```
    NAME               READY   UP-TO-DATE   AVAILABLE   AGE
    nginx-deployment   3/3     3            3           4m50s
    ```


## Get details of your Deployment:

```
kubectl describe deployments
```
The output is similar to this:
```
ubuntu@ubuntu-V5-171:~/github/kubernetes/examples$ kubectl describe deployments
Name:                   nginx-deployment
Namespace:              default
CreationTimestamp:      Mon, 13 Jul 2020 18:50:14 -0400
Labels:                 app=nginx
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=nginx
Replicas:               3 desired | 3 updated | 3 total | 3 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=nginx
  Containers:
   nginx:
    Image:        nginx:1.14.2
    Port:         80/TCP
    Host Port:    0/TCP
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   nginx-deployment-6b474476c4 (3/3 replicas created)
Events:
  Type    Reason             Age    From                   Message
  ----    ------             ----   ----                   -------
  Normal  ScalingReplicaSet  6m49s  deployment-controller  Scaled up replica set nginx-deployment-6b474476c4 to 3
  ```



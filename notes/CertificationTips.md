# Certification Tip: Imperative Commands

While you would be working mostly the declarative way - using definition files, imperative commands can help in getting one time tasks done quickly, as well as generate a definition template easily. This would help save a considerable amount of time during your exams.

Before we begin, familiarize with the two options that can come in handy while working with the below commands:

--dry-run: By default as soon as the command is run, the resource will be created. If you simply want to test your command, use the --dry-run=client option. This will not create the resource, instead, tell you whether the resource can be created and if your command is right.

-o yaml: This will output the resource definition in YAML format on the screen.



Use the above two in combination to generate a resource definition file quickly, that you can then modify and create resources as required, instead of creating the files from scratch.



## POD

Create an NGINX Pod
```
kubectl run nginx --image=nginx
```


Generate POD Manifest YAML file (-o yaml). Don't create it(--dry-run)
```
kubectl run nginx --image=nginx  --dry-run=client -o yaml
```


## Deployment
Create a deployment
```
kubectl create deployment --image=nginx nginx
```

``` 
kubectl create deployment weapp --image=kodekloud/webapp-color 
Generate Deployment YAML file (-o yaml). Don't create it(--dry-run)

kubectl create deployment --image=nginx nginx --dry-run=client -o yaml

# IMPORTANT:

kubectl create deployment does not have a --replicas option. You could first create it and then scale it using the kubectl scale command.



Save it to a file - (If you need to modify or add some other details)
```
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml
```
You can then update the YAML file with the replicas or any other field before creating the deployment.



## Service
Create a Service named redis-service of type ClusterIP to expose pod redis on port 6379
```
kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml
```
(This will automatically use the pod's labels as selectors)

Or
```
kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml  
```
(This will not use the pods labels as selectors, instead it will assume selectors as app=redis. You cannot pass in selectors as an option. So it does not work very well if your pod has a different label set. So generate the file and modify the selectors before creating the service)



Create a Service named nginx of type NodePort to expose pod nginx's port 80 on port 30080 on the nodes:
```
kubectl expose pod nginx --port=80 --name nginx-service --type=NodePort --dry-run=client -o yaml
```
(This will automatically use the pod's labels as selectors, but you cannot specify the node port. You have to generate a definition file and then add the node port in manually before creating the service with the pod.)

Or
```
kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml
```
(This will not use the pods labels as selectors)

Both the above commands have their own challenges. While one of it cannot accept a selector the other cannot accept a node port. I would recommend going with the `kubectl expose` command. If you need to specify a node port, generate a definition file using the same command and manually input the nodeport before creating the service.



Reference:

https://kubernetes.io/docs/reference/kubectl/conventions/

# Certification Tip: Formatting Output with kubectl

The default output format for all kubectl commands is the human-readable plain-text format.

The -o flag allows us to output the details in several different formats.

```
kubectl [command] [TYPE] [NAME] -o <output_format>
```
Here are some of the commonly used formats:

-o jsonOutput a JSON formatted API object.

-o namePrint only the resource name and nothing else.

-o wideOutput in the plain-text format with any additional information.

-o yamlOutput a YAML formatted API object.

Here are some useful examples:

* Output with JSON format:

        master $ kubectl create namespace test-123 --dry-run -o json
        {
            "kind": "Namespace",
            "apiVersion": "v1",
            "metadata": {
                "name": "test-123",
                "creationTimestamp": null
            },
            "spec": {},
            "status": {}
        }
        master $
* Output with YAML format:

        master $ kubectl create namespace test-123 --dry-run -o yaml
        apiVersion: v1
        kind: Namespace
        metadata:
        creationTimestamp: null
        name: test-123
        spec: {}
        status: {}
* Output with wide (additional details):

Probably the most common format used to print additional details about the object:

        master $ kubectl get pods -o wide
        NAME      READY   STATUS    RESTARTS   AGE     IP          NODE     NOMINATED NODE   READINESS GATES
        busybox   1/1     Running   0          3m39s   10.36.0.2   node01   <none>           <none>
        ningx     1/1     Running   0          7m32s   10.44.0.1   node03   <none>           <none>
        redis     1/1     Running   0          3m59s   10.36.0.1   node01   <none>           <none>
        master $
For more details, refer:

https://kubernetes.io/docs/reference/kubectl/overview/

https://kubernetes.io/docs/reference/kubectl/cheatsheet/



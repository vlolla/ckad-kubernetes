# Labels and Selectors

Labels are key/value pairs that are attached to objects, such as pods. Labels are intended to be used to specify identifying attributes of objects that are meaningful and relevant to users, but do not directly imply semantics to the core system.


For example, here’s the configuration file for a Pod that has two labels environment: production and app: nginx :

```
apiVersion: v1
kind: Pod
metadata:
  name: label-demo
  labels:
    environment: production
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80

```

Replica Set example

```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
    name: simple-webapp
        labels:
            app: App1
            function: Front-end
spec:
    replicas: 3
    selector:
        matchLabels:
            app: App1
    template:
        metadata:
            labels:
                app: App1
                function: Front-end
        spec:   
            containers:
                - name: simple-webapp
                image: simple-webapp
```
## Annotations

You can use Kubernetes annotations to attach arbitrary non-identifying metadata to objects. Clients such as tools and libraries can retrieve this metadata.
Attaching metadata to objects
```
annotations:
    key : value1
```

e.g

```
apiVersion: v1
kind: Pod
metadata:
  name: annotations-demo
  annotations:
    imageregistry: "https://hub.docker.com/"
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
```

## Selector

Both label selector styles can be used to list or watch resources via a REST client. For example, targeting apiserver with kubectl and using equality-based one may write:

    kubectl get pods -l environment=production,tier=frontend

or using set-based requirements:

    kubectl get pods -l 'environment in (production),tier in (frontend)'


'kubectl get pods --selector env=dev

'kubectl get all --selector env=prod


Identify the POD which is 'prod', part of 'finance' BU and is a 'frontend' tier?

kubectl get all --selector env=prod,bu=finance,tier=frontend
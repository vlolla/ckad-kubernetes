# ConfigMaps

A ConfigMap is an API object used to store non-confidential data in key-value pairs. Pods can consume ConfigMaps as environment variables, command-line arguments, or as configuration files in a volume.

A ConfigMap allows you to decouple environment-specific configuration from your container images, so that your applications are easily portable.

There are four different ways that you can use a ConfigMap to configure a container inside a Pod:

* Command line arguments to the entrypoint of a container
* Environment variables for a container
* Add a file in read-only volume, for the application to read
* Write code to run inside the Pod that uses the Kubernetes API to read a ConfigMap

These different methods lend themselves to different ways of modeling the data being consumed. For the first three methods, the kubelet uses the data from the ConfigMap when it launches container(s) for a Pod.


### Impertive way of creating a configmap

        kubectl create configmap <<config-name>> --from-literal=<key>=<value>

        e.g
        kubectl create configmap webapp-config-map --from-literal=APP_COLOR=darkblue

or From file

        ubectl create configmap <<config-name>> --from-file=<file-path>

        e.g
        kubectl create configmap webapp-config-map --from-file=app_config.properties

### Declarative Approach

Create a definition file for config map like we do for pods.. instead of spec we will have data compare to pod definition.

config-map.yml
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
  labels:
    app: web-color
data:
  APP_COLOR: blue
```
```
ubuntu@ubuntu-V5-171:~/github/kubernetes/examples/configurations$ kubectl create -f configmaps.yml 
configmap/app-config created
```

### View configmaps

```
ubuntu@ubuntu-V5-171:~/github/kubernetes/examples/configurations$ kubectl get configmaps
NAME                DATA   AGE
app-config          1      93s
webapp-config-map   1      11m
```

To describe config maps lists all the configmap and the data associated with configmaps in the namespace.

``` 
kubectl describe configmaps
```
```
ubuntu@ubuntu-V5-171:~/github/kubernetes/examples/configurations$ kubectl describe configmaps
Name:         app-config
Namespace:    default
Labels:       app=web-color
Annotations:  <none>

Data
====
APP_COLOR:
----
blue
Events:  <none>


Name:         webapp-config-map
Namespace:    default
Labels:       <none>
Annotations:  <none>

Data
====
APP_COLOR:
----
darkblue
Events:  <none>
ubuntu@ubuntu-V5-171:~/github/kubernetes/examples/configurations$ 
```


## ConfigMap in Pods

Simple pod defintion file with config map

```
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp-color
  labels:
    name: simple-webapp-color
spec:
  containers:
    - name: simple-webapp-color
      image: simple-webapp-color
      ports:
        - containerPort: 8080
      envFrom:
        - configMapRef:
            name: app-config
```

To ingest enviornment configs we have added new property called envFrom and provided the reference of the config map name which we have defined before as part of configmap.yml which is "app-config"

Inject Enviornment Values into Pods

```
envFrom:
    - configMapRef:
        name: app-config
```

Single Environment Variable

```
env:
  - name : APP_COLOR
    valueFrom:
        configMapRef:
            name: app-config
            key: APP COLOR
```

Volumes 

```
volumes:
- name : app-config-volume
  configMaps: 
    name: app-config

```


# Multi Pod Containers

To create a multi-container pod, add the new container information to the pod-
definition file. Remember, the containers section under the spec section in a pod
definition file is an array and the reason it is an array is to allow multiple containers in
a single POD. In this case we add a new container named log-agent to our existing
pod. We will look at more realistic examples later.

```
apiVersion: v1
kind: Pod
metadata:
name: simple-webapp
labels:
name: simple-webapp
spec:
containers:
- name: simple-webapp
image: simple-webapp
ports:
- containerPort: 8080
- name: log-agent
image: log-agent
```

## Design Patterns

There are 3 common patterns, when it comes to designing multi-container PODs. The
first above logging service example is known as a side car
pattern.

The others are the adapter and the ambassador pattern.

A good example of a side car pattern is deploying a logging agent along side a web
server to collect logs and forward them to a central log server.

So your application communicates to different database instances at different stages
of development. A local database for development, one for testing and another for
production. You must ensure to modify this connectivity depending on the
environment you are deploying your application to.

```
apiVersion: v1
kind: Pod
metadata:
name: simple-webapp
labels:
name: simple-webapp
spec:
containers:
- name: simple-webapp
image: simple-webapp
ports:
- containerPort: 8080
- name: log-agent
image: log-agent
```
Again, remember that these are different patterns in designing a multi-container pod.
When it comes to implementing them using a pod-definition file, it is always the
same. You simply have multiple containers within the pod definition file.

References

https://kubernetes.io/docs/tasks/access-application-cluster/communicate-containers-same-pod-
shared-volume/

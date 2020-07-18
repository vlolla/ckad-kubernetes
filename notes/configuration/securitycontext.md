# Security Context 

Docker Security

USER 1000

docker run --user=1001 ubuntu sleep 3600

## Configure a Security Context for a Pod or Container

### Set the security context for a Pod 

To specify security settings for a Pod, include the securityContext field in the Pod specification. 

The securityContext field is a PodSecurityContext object. The security settings that you specify for a Pod apply to all Containers in the Pod. 

Here is a configuration file for a Pod that has a securityContext and an emptyDir volume:

./examples/configuration/securitycontext.md
```
apiVersion: v1
kind: Pod
metadata:
  name: ubuntu
spec:
  securityContext:
    runAsUser: 1000
  containers:
    - name: ubuntu
      image: ubuntu
      command: 
          - "sleep"
          - "2000"
```

### Set the security context for a Container

To specify security settings for a Container, include the securityContext field in the Container manifest. The securityContext field is a SecurityContext object. Security settings that you specify for a Container apply only to the individual Container, and they override settings made at the Pod level when there is overlap. Container settings do not affect the Pod's Volumes.

Here is the configuration file for a Pod that has one Container. Both the Pod and the Container have a securityContext field:

```
apiVersion: v1
kind: Pod
metadata:
  name: ubuntu
spec:
  securityContext:
    runAsUser: 1000
  containers:
    - name: ubuntu
      image: ubuntu
      command: 
          - "sleep"
          - "2000"
      securityContext:
        runAsUser: 1000
        capabilities:
          add: ["MAC_ADMIN"]
```

Create a pod

        ubuntu@ubuntu-V5-171:~/github/kubernetes/examples/configurations$ kubectl apply -f securitycontext.yml 

        pod/ubuntu created

Get a shell into the running Container:

```
ubuntu@ubuntu-V5-171:~/github/kubernetes/examples/configurations$ kubectl exec -it ubuntu -- sh
$ ps aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
1000         1  0.0  0.0   2512   528 ?        Ss   22:57   0:00 sleep 2000
1000         6  0.2  0.0   2612   548 pts/0    Ss   22:58   0:00 sh
1000        12  0.0  0.0   5884  2856 pts/0    R+   22:58   0:00 ps aux
```

### Set capabilities for a Container

With Linux capabilities, you can grant certain privileges to a process without granting all the privileges of the root user. To add or remove Linux capabilities for a Container, include the capabilities field in the securityContext section of the Container manifest.

Below is the example of linux capabilites added to container.

```
apiVersion: v1
kind: Pod
metadata:
  name: security-context-demo-4
spec:
  containers:
  - name: sec-ctx-4
    image: gcr.io/google-samples/node-hello:1.0
    securityContext:
      capabilities:
        add: ["NET_ADMIN", "SYS_TIME"]
```


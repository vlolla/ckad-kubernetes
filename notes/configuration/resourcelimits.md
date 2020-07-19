# Resource Limits

## Resource units in Kubernetes
### Meaning of CPU
Limits and requests for CPU resources are measured in cpu units. One cpu, in Kubernetes, is equivalent to 1 vCPU/Core for cloud providers and 1 hyperthread on bare-metal Intel processors.
### Meaning of Memory
Limits and requests for memory are measured in bytes. You can express memory as a plain integer or as a fixed-point integer using one of these suffixes: E, P, T, G, M, K. You can also use the power-of-two equivalents: Ei, Pi, Ti, Gi, Mi, Ki. For example, the following represent roughly the same value:

Here's an example. The following Pod has two Containers. Each Container has a request of 0.25 cpu and 64MiB (226 bytes) of memory. Each Container has a limit of 0.5 cpu and 128MiB of memory. You can say the Pod has a request of 0.5 cpu and 128 MiB of memory, and a limit of 1 cpu and 256MiB of memory.

```
apiVersion: v1
kind: Pod
metadata:
  name: frontend
spec:
  containers:
  - name: db
    image: mysql
    env:
    - name: MYSQL_ROOT_PASSWORD
      value: "password"
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
  - name: wp
    image: wordpress
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
```



# Note on default resource requirements and limits

In the previous lecture, I said - "When a pod is created the containers are assigned a default CPU request of .5 and memory of 256Mi". For the POD to pick up those defaults you must have first set those as default values for request and limit by creating a LimitRange in that namespace.


```
    apiVersion: v1
    kind: LimitRange
    metadata:
    name: mem-limit-range
    spec:
    limits:
    - default:
        memory: 512Mi
        defaultRequest:
        memory: 256Mi
        type: Container
```
https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/memory-default-namespace/


```
    apiVersion: v1
    kind: LimitRange
    metadata:
    name: cpu-limit-range
    spec:
    limits:
    - default:
        cpu: 1
        defaultRequest:
        cpu: 0.5
        type: Container

```
https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/cpu-default-namespace/



References:

https://kubernetes.io/docs/tasks/configure-pod-container/assign-memory-resource
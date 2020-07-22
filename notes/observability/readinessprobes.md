# Liveness, Readiness and Startup Probes

## Liveness Probes

Pod conditions

   * PodScheduled
* Initialized
 *   ContainerReady
  *  Ready

To see the conditions of a pod

        kubectl get pods

Many applications running for long periods of time eventually transition to broken states, and cannot recover except by being restarted. Kubernetes provides liveness probes to detect and remedy such situations.

In this exercise, you create a Pod that runs a container based on the k8s.gcr.io/busybox image. Here is the configuration file for the Pod:

There are three ways to configure liveness

1. http
2. tcp
3. command

### Command livenessProbe - command

```
livenessProbe:
      exec:
        command:
        - cat
        - /tmp/healthy
      initialDelaySeconds: 5
      periodSeconds: 5
```

### http livenessProbe - http

```
livenessProbe:
      httpGet:
        path: /healthz
        port: 8080
        httpHeaders:
        - name: Custom-Header
          value: Awesome
      initialDelaySeconds: 3
      periodSeconds: 3
```
### TCP liveness probe

```
livenessProbe:
      tcpSocket:
        port: 8080
      initialDelaySeconds: 15
      periodSeconds: 20
```
### Use a named port

You can use a named ContainerPort for HTTP or TCP liveness checks:

    ports:
    - name: liveness-port
    containerPort: 8080
    hostPort: 8080

    livenessProbe:
    httpGet:
        path: /healthz
        port: liveness-port


## Protect slow starting containers with startup probes

Sometimes, you have to deal with legacy applications that might require an additional startup time on their first initialization. In such cases, it can be tricky to set up liveness probe parameters without compromising the fast response to deadlocks that motivated such a probe. The trick is to set up a startup probe with the same command, HTTP or TCP check, with a failureThreshold * periodSeconds long enough to cover the worse case startup time.

So, the previous example would become:

    ports:
    - name: liveness-port
    containerPort: 8080
    hostPort: 8080

    livenessProbe:
    httpGet:
        path: /healthz
        port: liveness-port
    failureThreshold: 1
    periodSeconds: 10

    startupProbe:
    httpGet:
        path: /healthz
        port: liveness-port
    failureThreshold: 30
    periodSeconds: 10


## Readiness probes

Readiness probes are configured similarly to liveness probes. The only difference is that you use the readinessProbe field instead of the livenessProbe field.
Note: Readiness probes runs on the container during its whole lifecycle.

    readinessProbe:
    exec:
        command:
        - cat
        - /tmp/healthy
    initialDelaySeconds: 5
    periodSeconds: 5

Configuration for HTTP and TCP readiness probes also remains identical to liveness probes.

initialDelaySeconds
periodSeconds
timeoutSeconds
successThreshold
failureThreshold

HTTP probes

host
scheme - default http
path
httpHeaders
port


# References
https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/

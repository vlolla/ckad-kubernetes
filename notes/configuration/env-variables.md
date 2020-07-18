# Environment Variables in Kubernetes

Define Environment Variables for a Container

When you create a Pod, you can set environment variables for the containers that run in the Pod. To set environment variables, include the env or envFrom field in the configuration file.

In this exercise, you create a Pod that runs one container. The configuration file for the Pod defines an environment variable with name DEMO_GREETING and value "Hello from the environment". Here is the configuration manifest for the Pod:

```
apiVersion: v1
kind: Pod
metadata:
  name: envar-demo
  labels:
    purpose: demonstrate-envars
spec:
  containers:
  - name: envar-demo-container
    image: gcr.io/google-samples/node-hello:1.0
    env:
    - name: DEMO_GREETING
      value: "Hello from the environment"
    - name: DEMO_FAREWELL
      value: "Such a sweet sorrow"
```
1. Create a Pod based on that manifest:

        kubectl apply -f env-varialbles.yml
2. List the running Pods:

        kubectl get pods -l purpose=demonstrate-envars
        
The output is similar to:

        NAME            READY     STATUS    RESTARTS   AGE
        envar-demo      1/1       Running   0          9s
3.  Get a shell to the container running in your Pod:

        kubectl exec -it envar-demo -- /bin/bash

In your shell, run the printenv command to list the environment variables.


```
root@envar-demo:/# printenv
NODE_VERSION=4.4.2
HOSTNAME=envar-demo
KUBERNETES_PORT=tcp://10.96.0.1:443
KUBERNETES_PORT_443_TCP_PORT=443
TERM=xterm
KUBERNETES_SERVICE_PORT=443
KUBERNETES_SERVICE_HOST=10.96.0.1
NPM_CONFIG_LOGLEVEL=info
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
PWD=/
DEMO_FAREWELL=Such a sweet sorrow
SHLVL=1
HOME=/root
KUBERNETES_PORT_443_TCP_PROTO=tcp
KUBERNETES_SERVICE_PORT_HTTPS=443
KUBERNETES_PORT_443_TCP_ADDR=10.96.0.1
KUBERNETES_PORT_443_TCP=tcp://10.96.0.1:443
DEMO_GREETING=Hello from the environment
_=/usr/bin/printenv
```

5. To exit the shell, enter exit.

##  Using environment variables inside of your config

Environment variables that you define in a Pod's configuration can be used elsewhere in the configuration, for example in commands and arguments that you set for the Pod's containers. In the example configuration below, the GREETING, HONORIFIC, and NAME environment variables are set to Warm greetings to, The Most Honorable, and Kubernetes, respectively. Those environment variables are then used in the CLI arguments passed to the env-print-demo container.

```
apiVersion: v1
kind: Pod
metadata:
  name: print-greeting
spec:
  containers:
  - name: env-print-demo
    image: bash
    env:
    - name: GREETING
      value: "Warm greetings to"
    - name: HONORIFIC
      value: "The Most Honorable"
    - name: NAME
      value: "Kubernetes"
    command: ["echo"]
    args: ["$(GREETING) $(HONORIFIC) $(NAME)"]
```

Upon creation, the command echo Warm greetings to The Most Honorable Kubernetes is run on the container.


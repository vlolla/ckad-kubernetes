# Ingress

What is Ingress?
Ingress exposes HTTP and HTTPS routes from outside the cluster to services within the cluster. Traffic routing is controlled by rules defined on the Ingress resource.

          internet
             |
        [ Ingress ]
        --|-----|--
        [ Services ]


An Ingress may be configured to give Services externally-reachable URLs, load balance traffic, terminate SSL / TLS, and offer name based virtual hosting. An Ingress controller is responsible for fulfilling the Ingress, usually with a load balancer, though it may also configure your edge router or additional frontends to help handle the traffic.

An Ingress does not expose arbitrary ports or protocols. Exposing services other than HTTP and HTTPS to the internet typically uses a service of type Service.Type=NodePort or Service.Type=LoadBalancer.


## Prerequisites
You must have an ingress controller to satisfy an Ingress. Only creating an Ingress resource has no effect.

You may need to deploy an Ingress controller such as ingress-nginx. You can choose from a number of Ingress controllers.

Ideally, all Ingress controllers should fit the reference specification. In reality, the various Ingress controllers operate slightly differently.

## Ingress Controller

Kubernetes as a project currently supports and maintains GCE and nginx controllers.

### Enable the Ingress controller

1. To enable the NGINX Ingress controller, run the following command:

        minikube addons enable ingress

2. Verify that the NGINX Ingress controller is running

        kubectl get pods -n kube-system

### Deploy a hello, world app

1. Deploy a hello, world app

        kubectl create deployment web --image=gcr.io/google-samples/hello-app:1.0

        deployment.apps/web created

2. Expose the Deployment:  Make it as a service

        kubectl expose deployment web --type=NodePort --port=8080

        service/web exposed
3. Verify the Service is created and is available on a node port:

        kubectl get service web

        ubuntu@ubuntu-V5-171:~/git/kubernetes$ kubectl get service web
        NAME   TYPE       CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
        web    NodePort   10.108.104.143   <none>        8080:30973/TCP   38s

4. Visit the service via NodePort:

        
       ubuntu@ubuntu-V5-171:~/git/kubernetes$ minikube service web --url --namespace=codehub32
http://172.17.0.3:30973

## Create an Ingress resource

The following file is an Ingress resource that sends traffic to your Service via hello-world.info.

1. Create example-ingress.yaml from the following file:

        ubuntu@ubuntu-V5-171:~/git/kubernetes/examples/services&networking$ k apply -f create-ingressresouce.yml 
        
        ingress.networking.k8s.io/example-ingress created
2. Verify the IP address is set:

        kubectl get ingress

        ubuntu@ubuntu-V5-171:~/git/kubernetes/examples/services&networking$ kubectl get ingress
        NAME              CLASS    HOSTS              ADDRESS      PORTS   AGE
        example-ingress   <none>   hello-world.info   172.17.0.3   80      107s
3. Add the following line to the bottom of the /etc/hosts file.

        172.17.0.3 hello-world.info

        ubuntu@ubuntu-V5-171:~/git/kubernetes$ curl hello-world.info
        Hello, world!
        Version: 1.0.0
        Hostname: web-6785d44d5-pfvnt


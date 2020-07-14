# Namespaces

Kubernetes supports multiple virtual clusters backed by the same physical cluster. These virtual clusters are called namespaces.

## Viewing namespaces

You can list the current namespaces in a cluster using:
```
kubectl get namespace
```

To list the pods the given namespace

```
kubectl get pods namespace=kube-system
```

## Setting the namespace for a request

To set the namespace for a current request, use the --namespace flag.

For example:

        kubectl run nginx --image=nginx --namespace=<insert-namespace-name-here>

        kubectl get pods --namespace=<insert-namespace-name-here>


# Namespaces and DNS

When you create a Service, it creates a corresponding DNS entry. This entry is of the form service-name.namespace-name.svc.cluster.local, which means that if a container just uses service-name, it will resolve to the service which is local to a namespace. This is useful for using the same configuration across multiple namespaces such as Development, Staging and Production. If you want to reach across namespaces, you need to use the fully qualified domain name (FQDN)


## Create a Namespace

1. Create a new YAML file called namespace-dev.yml with the contents:

        apiVersion: v1
        kind: Namespace
        metadata:
            name: dev
    Then run:

    
        kubectl create -f ./my-namespace.yaml

2. Alternative we can give imperative commands
    ```
    kubectl create namespace dev
    ```
# Deleting a namespace

Delete a namespace with

        kubectl delete namespaces dev

Warning: This deletes everything under the namespace!

This delete is asynchronous, so for a time you will see the namespace in the Terminating state

## Setting the namespace preference

You can permanently save the namespace for all subsequent kubectl commands in that context.

    kubectl config set-context --current --namespace=dev
    validate uisng below command
    kubectl config view --minify | grep namespace:
    
## View pods from all names spaces

    kubectl get pods --all-namespaces


# Resource Quota




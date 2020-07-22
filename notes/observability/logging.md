# Logging

Viewing Logs in Kubernetes

    kubectl logs -f pod

If multiple containers on a pod then we must specify container name

    kubectl logs -f pod-name container-name



# Volumes

At its core, a volume is just a directory, possibly with some data in it, which is accessible to the Containers in a Pod. How that directory comes to be, the medium that backs it, and the contents of it are determined by the particular volume type used.

Kubernetes supports many volumes types

https://kubernetes.io/docs/concepts/storage/volumes/#types-of-volumes

### AWS EBS Example configuration

```
apiVersion: v1
kind: Pod
metadata:
  name: test-ebs
spec:
  containers:
  - image: k8s.gcr.io/test-webserver
    name: test-container
    volumeMounts:
    - mountPath: /test-ebs
      name: test-volume
  volumes:
  - name: test-volume
    # This AWS EBS volume must already exist.
    awsElasticBlockStore:
      volumeID: <volume-id>
      fsType: ext4
```

## Persistent Volumes

Managing storage is a distinct problem from managing compute instances. The PersistentVolume subsystem provides an API for users and administrators that abstracts details of how storage is provided from how it is consumed. To do this, we introduce two new API resources: PersistentVolume and PersistentVolumeClaim.

A PersistentVolume (PV) is a piece of storage in the cluster that has been provisioned by an administrator or dynamically provisioned using Storage Classes. It is a resource in the cluster just like a node is a cluster resource. PVs are volume plugins like Volumes, but have a lifecycle independent of any individual Pod that uses the PV. This API object captures the details of the implementation of the storage, be that NFS, iSCSI, or a cloud-provider-specific storage system.

A PersistentVolumeClaim (PVC) is a request for storage by a user. It is similar to a Pod. Pods consume node resources and PVCs consume PV resources. Pods can request specific levels of resources (CPU and Memory). Claims can request specific size and access modes (e.g., they can be mounted ReadWriteOnce, ReadOnlyMany or ReadWriteMany, see AccessModes).

### Persistent Volume definition

```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: task-pv-volume
  labels:
    type: local
spec:
  storageClassName: manual
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"
```

Create PV using 

        ubuntu@ubuntu-V5-171:~/git/kubernetes/examples/persistentvolume$ k create -f persistentvolume-definition.yml 
        persistentvolume/pv0003 created

To view PV created

  
     kubectl get pv
        NAME     CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS   REASON   AGE
        pv0003   5Gi        RWO            Recycle          Available           slow                    65s


## Persistent Volume Claims

### Create a PersistentVolumeClaim

The next step is to create a PersistentVolumeClaim. Pods use PersistentVolumeClaims to request physical storage. In this exercise, you create a PersistentVolumeClaim that requests a volume of at least three gibibytes that can provide read-write access for at least one Node.

Here is the configuration file for the PersistentVolumeClaim:

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: task-pv-claim
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 3Gi
```

Create the PersistentVolumeClaim:

    kubectl apply -f ./examples/persistentvolume/pvclaim-def.yml


    ubuntu:~$ kubectl get pvc task-pv-claim --namespace=codehub32
    NAME            STATUS   VOLUME           CAPACITY   ACCESS MODES   STORAGECLASS   AGE
    task-pv-claim   Bound    task-pv-volume   10Gi       RWO            manual         4s

## Create a Pod

The next step is to create a Pod that uses your PersistentVolumeClaim as a volume.

```
apiVersion: v1
kind: Pod
metadata:
  name: task-pv-pod
spec:
  volumes:
    - name: task-pv-storage
      persistentVolumeClaim:
        claimName: task-pv-claim
  containers:
    - name: task-pv-container
      image: nginx
      ports:
        - containerPort: 80
          name: "http-server"
      volumeMounts:e
        - mountPath: "/usr/share/nginx/html"
          name: task-pv-storage
```

Create pod using kubectl apply

        ubuntu:~$ kubectl apply -f ./git/kubernetes/examples/persistentvolume/pod-pvc.yml --namespace=codehub32
        pod/task-pv-pod created


        ubuntu:~$ kubectl get pod task-pv-pod --namespace=codehub32
        NAME          READY   STATUS              RESTARTS   AGE
        task-pv-pod   0/1     ContainerCreating   0          20s
        ubuntu:~$ 
        ubuntu:~$ kubectl get pod task-pv-pod --namespace=codehub32
        NAME          READY   STATUS    RESTARTS   AGE
        task-pv-pod   1/1     Running   0          58s  


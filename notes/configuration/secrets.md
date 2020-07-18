## Secrets

Kubernetes Secrets let you store and manage sensitive information, such as passwords, OAuth tokens, and ssh keys. Storing confidential information in a Secret is safer and more flexible than putting it verbatim in a Pod definition or in a container image.

To use a secret, a Pod needs to reference the secret. A secret can be used with a Pod in three ways:

* As files in a volume mounted on one or more of its containers.
* As container environment variable.
* By the kubelet when pulling images for the Pod.


## Creating your own Secrets


### Creating a Secret Using kubectl

Secrets can contain user credentials required by Pods to access a database. For example, a database connection string consists of a username and password. You can store the username in a file ./username.txt and the password in a file ./password.txt on your local machine.

Imperative way:

        kubectl create secret  generic <secret-name> --from-literal=<key>=<value>

sample
```
        kubectl create secret generic db-secret --from-literal=DB_PWD=abcedfg
        --from literal=DB_HOST=localhost
        --from literal=DB_PORT=2001
```
through file

        kubectl create secret generic
            db-secret --from-file=dbconfig.properties

## Declerative approach needs a definition file 

We would create a definition file just like config map
```
apiVersion: v1
kind: Secret
metadata:
  name:  db-secret
data:
  DB_NAME: mysql
  DB_USER: rootuser
  DB_PWD: test123
```
### Encode Secrets

If you see in the data we should not give db data in plain test, so to convert into base64 encrypted we can use linux echo utility.

e.g

```
ubuntu@ubuntu-V5-171:~/github/kubernetes/examples/configurations$ echo -n "mysql" | base64
bXlzcWw=
ubuntu@ubuntu-V5-171:~/github/kubernetes/examples/configurations$ echo -n "rootuser" | base64
cm9vdHVzZXI=
ubuntu@ubuntu-V5-171:~/github/kubernetes/examples/configurations$ echo -n "test123" | base64
dGVzdDEyMw==
```

Now replace your secret-definition.yml with newly encoded data

```
apiVersion: v1
kind: Secret
metadata:
  name:  db-secret
data:
  DB_NAME: bXlzcWw=
  DB_USER: cm9vdHVzZXI=
  DB_PWD: dGVzdDEyMw==
```


### Create Secret

    ubuntu@ubuntu-V5-171:~/github/kubernetes/examples/configurations$ kubectl create -f  secrets-definition.yml 
    secret/db-secret created



### View Secrets

```
buntu@ubuntu-V5-171:~/github/kubernetes/examples/configurations$ kubectl get secret
NAME                  TYPE                                  DATA   AGE
db-secret             Opaque                                3      100s
default-token-qx492   kubernetes.io/service-account-token   3      5d4h
```

### Describe Secrets

```
ubuntu@ubuntu-V5-171:~/github/kubernetes/examples/configurations$ kubectl describe secret
Name:         db-secret
Namespace:    default
Labels:       <none>
Annotations:  <none>

Type:  Opaque

Data
====
DB_NAME:  5 bytes
DB_PWD:   7 bytes
DB_USER:  8 bytes
```

### View the value of the Secret

```
ubuntu@ubuntu-V5-171:~/github/kubernetes/examples/configurations$ kubectl get secret db-secret -o yaml
apiVersion: v1
data:
  DB_NAME: bXlzcWw=
  DB_PWD: dGVzdDEyMw==
  DB_USER: cm9vdHVzZXI=
kind: Secret
metadata:
  creationTimestamp: "2020-07-18T20:37:20Z"
  managedFields:
  - apiVersion: v1
    fieldsType: FieldsV1
    fieldsV1:
      f:data:
        .: {}
        f:DB_NAME: {}
        f:DB_PWD: {}
        f:DB_USER: {}
      f:type: {}
    manager: kubectl
    operation: Update
    time: "2020-07-18T20:37:20Z"
  name: db-secret
  namespace: default
  resourceVersion: "16983"
  selfLink: /api/v1/namespaces/default/secrets/db-secret
  uid: e08df3f6-6433-4a85-a103-b1bb07f8f5c9
type: Opaque
ubuntu@ubuntu-V5-171:~/github/kubernetes/examples/configurations$ 

```

## Decode Secrete

Use linux echo command but use decode to see values

```
ubuntu@ubuntu-V5-171:~/github/kubernetes/examples/configurations$ echo  -n 'bXlzcWw=' | base64 --decode 
mysql
ubuntu@ubuntu-V5-171:~/github/kubernetes/examples/configurations$ echo -n 'dGVzdDEyMw==' | base64 --decode
test123
ubuntu@ubuntu-V5-171:~/github/kubernetes/examples/configurations$ echo -n 'cm9vdHVzZXI='  | base64 --decode
rootuser

```

## Inject Secret into PODS

To inject the secret into pod definition file have envFrom variable.. refer to the secret-definition.yml for name as we defined.

```
envFrom:
  - secretRef:
        name: db-secret
```
Updated the above secret ref in createpodwithconfigmap.yml to take the secret and ran kubectl apply

```
ubuntu@ubuntu-V5-171:~/github/kubernetes/examples/configurations$ kubectl apply -f createpodwithconfigmap.yml 
pod/simple-webapp-color created
```

There are other ways to inject secret into pods as we do in config maps.

1. Env variable - as we discussed above

```
envFrom:
  - secretRef:
        name: db-secret
```

2. Single Env variable

```
env:
  - name: DB Password
    valueFrom:
        secretKeyRef:
            name: db-secret
            key: DB_PASSWORD
    
```

3. As Volumes - injest whole secrets as volume

```
volumes:
- name: db-secret-volume
  secret: 
    secretName: db-secret
```

Mount Secret as volume.

```
/opt/db-secret-volume
```

# A quick note about Secrets!

Remember that secrets encode data in base64 format. Anyone with the base64 encoded secret can easily decode it. As such the secrets can be considered as not very safe.

The concept of safety of the Secrets is a bit confusing in Kubernetes. The kubernetes documentation page and a lot of blogs out there refer to secrets as a "safer option" to store sensitive data. They are safer than storing in plain text as they reduce the risk of accidentally exposing passwords and other sensitive data. In my opinion it's not the secret itself that is safe, it is the practices around it. 

Secrets are not encrypted, so it is not safer in that sense. However, some best practices around using secrets make it safer. As in best practices like:

Not checking-in secret object definition files to source code repositories.

Enabling Encryption at Rest for Secrets so they are stored encrypted in ETCD. 



Also the way kubernetes handles secrets. Such as:

A secret is only sent to a node if a pod on that node requires it.

Kubelet stores the secret into a tmpfs so that the secret is not written to disk storage.

Once the Pod that depends on the secret is deleted, kubelet will delete its local copy of the secret data as well.

Read about the protections and risks of using secrets here



Having said that, there are other better ways of handling sensitive data like passwords in Kubernetes, such as using tools like Helm Secrets, HashiCorp Vault. I hope to make a lecture on these in the future.
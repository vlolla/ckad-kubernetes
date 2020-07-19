# Configure Service Accounts for Pods

* User Account

* Service Account

When you (a human) access the cluster (for example, using kubectl), you are authenticated by the apiserver as a particular User Account (currently this is usually admin, unless your cluster administrator has customized your cluster). Processes in containers inside pods can also contact the apiserver. When they do, they are authenticated as a particular Service Account (for example, default)


In version 1.6+, you can opt out of automounting API credentials for a service account by setting automountServiceAccountToken: false on the service account:

```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: build-robot
automountServiceAccountToken: false
```

you can also opt out of automounting API credentials for a particular pod:

```
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  serviceAccountName: build-robot
  automountServiceAccountToken: false
```
### Use Multiple Service Accounts

    kubectl get serviceaccounts 

    ubuntu@ubuntu-V5-171:~/github/kubernetes/examples$ kubectl get serviceaccounts
    NAME      SECRETS   AGE
    default   1         5d11h

## Create a Service Account

    kubectl create serviceaccount dashboard-sa

## Get token from service account
```
ubuntu@ubuntu-V5-171:~/github/kubernetes/examples$ kubectl describe secret dashboard-sa

Name:         dashboard-sa-token-6d9kp
Namespace:    default
Labels:       <none>
Annotations:  kubernetes.io/service-account.name: dashboard-sa
              kubernetes.io/service-account.uid: fc0dfcc8-c350-4649-b163-f0deb65111ae

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1066 bytes
namespace:  7 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6ImxoVU1vdGVrSmY2ZlVnbXVwWVR5WmhWVzJSb2R1eVlBVFNyOGM2clVEUG8ifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJkZWZhdWx0Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZWNyZXQubmFtZSI6ImRhc2hib2FyZC1zYS10b2tlbi02ZDlrcCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50Lm5hbWUiOiJkYXNoYm9hcmQtc2EiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiJmYzBkZmNjOC1jMzUwLTQ2NDktYjE2My1mMGRlYjY1MTExYWUiLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6ZGVmYXVsdDpkYXNoYm9hcmQtc2EifQ.H0XasNCm2xa9PKKAy2Yt1W9aT62ebdRgZEPckzIxR-j-0v4Y3ODKC2c4iPdZ3QCvu2aELxjDdOLvLogNWqi-LjzVzQmrkH1TlzqJ51I6ccyz0RTFFno2SKb775HjiL4kMs2VcnuhX52pFuq5zj90TTIpNbc6axvTkYXhynp2P_w-7O0nCBzHnIe0fb9J6uHm-9jxzpIouDBcmNQbUssf3ZB74gqh1mUIU43U8m9ug4B5PsSj7QfgZvUlKmz0qFDxO_JaLGcQXKJ0U1vOUHOrlMgXe64EVfSjR6KFT6Uxa1p3U9DKhZvpCZI-QaajGsBl_ua_yerjbxvhMp39z4E0Jg
```

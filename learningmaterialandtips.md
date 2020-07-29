# Learning Materials

1. Enroll in [Introduction to Kubernetes](https://www.edx.org/course/introduction-to-kubernetes)if you are new to K8S at EdX.org

2. CKAD from KodeKloud or Udemy  [Kubernetes Certified Application Developer (CKAD) with Tests](https://www.udemy.com/course/certified-kubernetes-application-developer/)
Learn concepts and practice for the Kubernetes Certification with hands-on labs right in your browser - DevOps - CKAD 

3. Kubernetes Learning Path which can be download [here](https://azure.microsoft.com/en-us/resources/kubernetes-learning-path/). It has 50 days from zero to hero learning plan, including videos, hands-on, documentation, etc. It cover the basic Kubernetes concept and uses [Azure Kubernetes Services](https://azure.microsoft.com/en-us/services/kubernetes-service/) as example. The content used in this learning path is mostly free. 

4. [CKAD Exercise](https://github.com/dgkanatsios/CKAD-exercises) nice repo for few exercises help to be fluent with the exam pattern.

5. More Exercises from https://medium.com/bb-tutorials-and-thoughts/practice-enough-with-these-questions-for-the-ckad-exam-2f42d1228552

6. https://kubernetesbyexample.com/  -- this has nice examples by topics

# Tips of passing the exam

1. vi editor fluency and practice -- https://devhints.io/vim

2. Kubernetes docs (https://kubernetes.io ) especially the example. Get familiar with the structure of the docs including the examples. So that you can copy-paste the appropriate example to your terminal with minimum modification.  

3. Familiar with Kubernetes’ object’s short names

```
$ kubectl api-resources
NAME                              SHORTNAMES   APIGROUP                       NAMESPACED   KIND
bindings                                                                      true         Binding
componentstatuses                 cs                                          false        ComponentStatus
configmaps                        cm                                          true         ConfigMap
endpoints                         ep                                          true         Endpoints
events                            ev                                          true         Event
limitranges                       limits                                      true         LimitRange
namespaces                        ns                                          false        Namespace
nodes                             no                                          false        Node
persistentvolumeclaims            pvc                                         true         PersistentVolumeClaim
persistentvolumes                 pv                                          false        PersistentVolume
pods                              po                                          true         Pod
podtemplates                                                                  true         PodTemplate
replicationcontrollers            rc                                          true         ReplicationController
resourcequotas                    quota                                       true         ResourceQuota
secrets                                                                       true         Secret
serviceaccounts                   sa                                          true         ServiceAccount
services                          svc                                         true         Service
mutatingwebhookconfigurations                  admissionregistration.k8s.io   false        MutatingWebhookConfiguration
validatingwebhookconfigurations                admissionregistration.k8s.io   false        ValidatingWebhookConfiguration
customresourcedefinitions         crd,crds     apiextensions.k8s.io           false        CustomResourceDefinition
apiservices                                    apiregistration.k8s.io         false        APIService
controllerrevisions                            apps                           true         ControllerRevision
daemonsets                        ds           apps                           true         DaemonSet
deployments                       deploy       apps                           true         Deployment
replicasets                       rs           apps                           true         ReplicaSet
statefulsets                      sts          apps                           true         StatefulSet
tokenreviews                                   authentication.k8s.io          false        TokenReview
localsubjectaccessreviews                      authorization.k8s.io           true         LocalSubjectAccessReview
selfsubjectaccessreviews                       authorization.k8s.io           false        SelfSubjectAccessReview
selfsubjectrulesreviews                        authorization.k8s.io           false        SelfSubjectRulesReview
subjectaccessreviews                           authorization.k8s.io           false        SubjectAccessReview
horizontalpodautoscalers          hpa          autoscaling                    true         HorizontalPodAutoscaler
cronjobs                          cj           batch                          true         CronJob
jobs                                           batch                          true         Job
certificatesigningrequests        csr          certificates.k8s.io            false        CertificateSigningRequest
leases                                         coordination.k8s.io            true         Lease
endpointslices                                 discovery.k8s.io               true         EndpointSlice
events                            ev           events.k8s.io                  true         Event
ingresses                         ing          extensions                     true         Ingress
ingressclasses                                 networking.k8s.io              false        IngressClass
ingresses                         ing          networking.k8s.io              true         Ingress
networkpolicies                   netpol       networking.k8s.io              true         NetworkPolicy
runtimeclasses                                 node.k8s.io                    false        RuntimeClass
poddisruptionbudgets              pdb          policy                         true         PodDisruptionBudget
podsecuritypolicies               psp          policy                         false        PodSecurityPolicy
clusterrolebindings                            rbac.authorization.k8s.io      false        ClusterRoleBinding
clusterroles                                   rbac.authorization.k8s.io      false        ClusterRole
rolebindings                                   rbac.authorization.k8s.io      true         RoleBinding
roles                                          rbac.authorization.k8s.io      true         Role
priorityclasses                   pc           scheduling.k8s.io              false        PriorityClass
csidrivers                                     storage.k8s.io                 false        CSIDriver
csinodes                                       storage.k8s.io                 false        CSINode
storageclasses                    sc           storage.k8s.io                 false        StorageClass
volumeattachments                              storage.k8s.io                 false        VolumeAttachment
```

Most important are these.
```
    no for nodes
    po for pods
    deploy for deployments
    rs for replicasets
    pv for persistentvolumes
    pvc for persitentvolumeclaims
    ns for namespaces
    rc for replicationcontroller
```

4. Create shortcuts/alias before you start the exam

```
alias k='kubectl'
alias kg='kubectl get'
alias kgpo='kubectl get pod'
```
see the full list [here](https://github.com/ahmetb/kubectl-aliases/blob/master/.kubectl_aliases)

5. Imperative follow by declarative (-o yaml –dry-run)

There are typically 2 ways to creating Kubernetes objects. Imperative way (thru kubectl) or declarative way (thru yaml then kubectl apply/create). Obviously, you don’t have to hand-write every single yaml lines, you may copy and paste the example from Kubernetes doc and modify them accordingly. 


–dry-run option is commonly used to validate if the command (and parameters) are correctly specify without really running it

-o yaml option is used to produce an output in yaml format


Assuming you are asked to create a deployment with 4 replicas, nginx image. You may do it as following:

    $ k create deploy nginx-deploy –image=nginx –dry-run -o yaml > nginx-deploy.yaml

This produce a yaml file named nginx-deploy.yaml. Then you use vi/vim to edit replicas option in the yaml file. This is because we are unable to specify the replica option in the imperative kubectl create deploy command. 


6. Careful with namespace

You can specify the namespace either thru imperative kubectl command with -n (or –namespace) option such as: 

```
ubuntu@ubuntu-V5-171:~/github$ kubectl create ns myns
namespace/myns created
```
```
ubuntu@ubuntu-V5-171:~/github$ k run ngnix --image=ngnix --generator=run-pod/v1 -n myns
Flag --generator has been deprecated, has no effect and will be removed in the future.
pod/ngnix created
```

Declaratively in metadata section as can be seen here: 

Lets take the same ngnix example and see how the namespace looks in yml

First lets set the context to current namespace to myns
```
kubectl config set-context --current --namespace=<insert-namespace-name-here>
```
```
ubuntu@ubuntu-V5-171:~/github$ k config set-context --current --namespace=myns
Context "minikube" modified.

ubuntu@ubuntu-V5-171:~/github$ kubectl get pod ngnix -o yaml > ngnixpod.yaml
```

Once you have the ngnixpod.yaml we can use it to change for any question we have in the test

```
ubuntu@ubuntu-V5-171:~/github$ cat ngnixpod.yaml 
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: "2020-07-19T22:08:57Z"
  labels:
    run: ngnix
  managedFields:
  - apiVersion: v1
    fieldsType: FieldsV1
    fieldsV1:
      f:metadata:
        f:labels:
          .: {}
          f:run: {}
      f:spec:
        f:containers:
          k:{"name":"ngnix"}:
            .: {}
            f:image: {}
            f:imagePullPolicy: {}
            f:name: {}
            f:resources: {}
            f:terminationMessagePath: {}
            f:terminationMessagePolicy: {}
        f:dnsPolicy: {}
        f:enableServiceLinks: {}
        f:restartPolicy: {}
        f:schedulerName: {}
        f:securityContext: {}
        f:terminationGracePeriodSeconds: {}
    manager: kubectl
    operation: Update
    time: "2020-07-19T22:08:57Z"
  - apiVersion: v1
    fieldsType: FieldsV1
    fieldsV1:
      f:status:
        f:conditions:
          k:{"type":"ContainersReady"}:
            .: {}
            f:lastProbeTime: {}
            f:lastTransitionTime: {}
            f:message: {}
            f:reason: {}
            f:status: {}
            f:type: {}
          k:{"type":"Initialized"}:
            .: {}
            f:lastProbeTime: {}
            f:lastTransitionTime: {}
            f:status: {}
            f:type: {}
          k:{"type":"Ready"}:
            .: {}
            f:lastProbeTime: {}
            f:lastTransitionTime: {}
            f:message: {}
            f:reason: {}
            f:status: {}
            f:type: {}
        f:containerStatuses: {}
        f:hostIP: {}
        f:podIP: {}
        f:podIPs:
          .: {}
          k:{"ip":"172.18.0.13"}:
            .: {}
            f:ip: {}
        f:startTime: {}
    manager: kubelet
    operation: Update
    time: "2020-07-19T22:15:17Z"
  name: ngnix
  namespace: myns
  resourceVersion: "26117"
  selfLink: /api/v1/namespaces/myns/pods/ngnix
  uid: 3192aba7-61b8-411a-8e3d-485036566653
spec:
  containers:
  - image: ngnix
    imagePullPolicy: Always
    name: ngnix
    resources: {}
    terminationMessagePath: /dev/termination-log
    terminationMessagePolicy: File
    volumeMounts:
    - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
      name: default-token-vgjgf
      readOnly: true
  dnsPolicy: ClusterFirst
  enableServiceLinks: true
  nodeName: minikube
  priority: 0
  restartPolicy: Always
  schedulerName: default-scheduler
  securityContext: {}
  serviceAccount: default
  serviceAccountName: default
  terminationGracePeriodSeconds: 30
  tolerations:
  - effect: NoExecute
    key: node.kubernetes.io/not-ready
    operator: Exists
    tolerationSeconds: 300
  - effect: NoExecute
    key: node.kubernetes.io/unreachable
    operator: Exists
    tolerationSeconds: 300
  volumes:
  - name: default-token-vgjgf
    secret:
      defaultMode: 420
      secretName: default-token-vgjgf
status:
  conditions:
  - lastProbeTime: null
    lastTransitionTime: "2020-07-19T22:08:57Z"
    status: "True"
    type: Initialized
  - lastProbeTime: null
    lastTransitionTime: "2020-07-19T22:08:57Z"
    message: 'containers with unready status: [ngnix]'
    reason: ContainersNotReady
    status: "False"
    type: Ready
  - lastProbeTime: null
    lastTransitionTime: "2020-07-19T22:08:57Z"
    message: 'containers with unready status: [ngnix]'
    reason: ContainersNotReady
    status: "False"
    type: ContainersReady
  - lastProbeTime: null
    lastTransitionTime: "2020-07-19T22:08:57Z"
    status: "True"
    type: PodScheduled
  containerStatuses:
  - image: ngnix
    imageID: ""
    lastState: {}
    name: ngnix
    ready: false
    restartCount: 0
    started: false
    state:
      waiting:
        message: Back-off pulling image "ngnix"
        reason: ImagePullBackOff
  hostIP: 172.17.0.2
  phase: Pending
  podIP: 172.18.0.13
  podIPs:
  - ip: 172.18.0.13
  qosClass: BestEffort
  startTime: "2020-07-19T22:08:57Z"
ubuntu@ubuntu-V5-171:~/github$ 
```
# Rolling Updates and Deployment Strategy

Definition

        > kubectl create –f deployment-definition.yml
        deployment "myapp-deployment" created
        > kubectl get deployments
        NAME DESIRED CURRENT UP-TO-DATE AVAILABLE AGE
        myapp-deployment 3 3 3 3 21s
        > kubectl get replicaset
        NAME DESIRED CURRENT READY AGE
        myapp-deployment-6795844b58 3 3 3 2m


# kubectl

syntax: kubectl <action> <resource>
.kube file in home directory contains the configuration of the service and is read on every call

## Create deployment
- kubectl create deployment <deploy_name> --image=<image>
- kubectl create deployment hello-node --image=k8s.gcr.io/echoserver:1.4
- kubectl create deployment pingpong --image alpine -- ping 1.1.1.1
- kubectl apply -f <resource>.yaml
    options:
        -f: file
        -k
## list the pods in the cluster
- kubectl get pods -A
- kubectl get pods --all-namespaces
- kubectl get pods -n <namespace>

## list the nodes in the cluster
- kubectl get nodes
    options:
        --config | --server | --user        # custom specifications
        -o wide                             # Get more information
        -o yml                              # output in yml

## Get info
- kubectl get all (hierarchy: deployment -> replicaset -> pod  |  service is independent)
- kubectl describe <resource> <instance>              # Description of the resource (useful debug errors)
- kubectl explain <resource> <instance>.<attribute>   # Get the definition of the resource
- kubectl explain <resource> --recursive              # Verbose explanation
- kubectl logs <pod>|<deploy/deploy_name> --tail n
- kubectl logs -l <label_name>                        # The label_name could be found with describe


## Secrets
- kubectl get secrets -n <namespace>
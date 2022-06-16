# Namespaces
- Diferentiates the resources in the sandbox
- kubectl get namespace

# create namespace
- kubectl create namespace <name>

# select a namespace for an specific action
- kubectl -n <namespace> <action> <resource>
- kubectl -n bitcoin get services

# set the current context (switch to a namespace)
- kubectl config set-context --current --namespace=<namespace>

# get the current namespace
- kubectl config get-contexts

# visibility is given by:
- namespace
- resource type
- resource name
**This attributes uniquely define a kubernetes resource**


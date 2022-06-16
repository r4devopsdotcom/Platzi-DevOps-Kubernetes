# Deploy multiple instances of the same app in a cluster
Use cases:
- Deploy multiple versions of the same application
- Have multiple environments (dev, staging, main)
- Increase availability

# Change namespace
- kubectl create namespace blue
- kubectl get namespace
- kubectl set-context --current --namespace=<namespace>
- kubectl set-context --current --namespace=blue

# Deploy the second app in the new namespace
- helm install <chart_name>
- helm install bitcoin

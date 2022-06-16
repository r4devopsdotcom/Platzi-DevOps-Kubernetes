# Deployments
It's a high level structure

Handle:
- Scaling (from high level, like an interface)
- Rolling-ups
- Pod version handling
- Rollbacks
- canary deployment (deploy app v.1.0 & v.2.0, then redirect a part of the traffic to the new version)

# Replica sets
Its the observer of the cluster

- Ensure that the defined number of pods are running
- Scaling (the deployments gives the order and the replica set execute it)

# Scaling
- kubectl scale <deploy/name> --replicas <n>
- result: kubectl get pods -o wide

# Observability
- kubectl get pods -w           # w: watching

# Get the manifest file
- kubectl run --dry-run -o yaml <pod_name> --image <image>
- kubectl run --dry-run -o yaml pingpong --image alpine ping 1.1.1.1            # example

# Delete deployment
- kubectl delete deploy/<name>
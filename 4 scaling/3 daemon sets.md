# Daemon Sets
Allow conditional scaling (must be created using manifest files)

# Objective
- Scale RNG service to have maximum 1 container per pod

# Select the target service, generate a spec with a DaemonSet, apply the generated spec
- kubectl get deploy/rng -o yaml --export > rng.yml             # --export is deprecated
- Update:
  - Kind: Deployment  --TO-->  Kind: DaemonSet
- kubectl apply -f rng.yml --validate=false                     # avoid warnings just for tests

# How Load Balancing Works?
Load balancing works using the labels of the pods, in this example,
the label of one pod is deleted and you can realize that kubectl
creates a new pod, assign the label and routes the traffic there.

- kubectl get pods --selector=app=rng -w
- kubectl label pod rng-<pod_uuid> app-                         # Delete the label of the pod
# Master Node
Elements
- API server: Is the brain of the cluster. Is connected to the etcd
- Scheduler
- Controller manager: Help the containers to reach their desired state.
- etcd: High availability database

# Nodes components
- Container runtime: docker
- Kubelet:
    - Is the Kubernetes agent that connects to the API server and ask for what the resources to run.
    - Also, keep the Controller manager up to date about the state of pods
- Kub-proxy:
    - Balance the service traffics
- Also called minions
- Pods in the container, share the physic network
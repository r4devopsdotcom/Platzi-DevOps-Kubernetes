# Master Node
Elements
- API server: Is the brain of the cluster. Is connected to the etcd
- Scheduler: Deploy pods in the nodes
- Controller manager: Help the containers to reach their desired state.
  - Types: Replica, Service, Deployment... managers
- etcd: High availability database

# Nodes components
- Container runtime: docker (containerd)
- Kubelet:
    - Is the Kubernetes agent that connects to the control plane (API server), ask for what resources should be run and then deploy them
    - Also, keep the control plane (API server) up to date about the state of pods
    - Run liveness probes
- Kub-proxy:
    - Balance service traffic
- Also called minions
- The Nodes share the physic network and it's recommended that they are visibile to each other
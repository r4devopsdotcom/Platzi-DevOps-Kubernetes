Docker: containers management
Kubernetes:
    - In charge of workload placement
        - Containers/pod communication
        - Nodes scaling
        - Pod: orchestration unit
    - Evolution of Borg & Omega (google projects)
    - Open Source (CNCF: Cloud Native Computing Foundation)
    - Enterprise and most cloud providers can run kubernetes (AWS, GCP, Azure, DigitalOcean)

# Core concepts
- Run replicas (pods/containers) and ensure their availability
- Provide a Load Balancer thanks to cloud providers
- Code roll-out mechanisms
- Scaling policies
- Jobs batch
- CRDs (custom resource definitions)
- Service catalog
- RBAC (rolebase access control)

# Pod
- They are a group of containers that lives in a Node
- Containers in the same pod, share the same namespace (network interface)

# Scaling
When a pod scales, it is replicated with all their containers inside
# Service Types

Cluster IP
- Assigns a virtual IP for the service (1 IP of the CIDR is assigned to a service)
- Allows the communication between containers and pods balancing the load using
  the information of the endpoints and labels in the services

Node Port
- Reserves a port to each node of the cluster
- Ports range of the cluster could be defined at the API server bootstrap

Load Balancer
- A load balancer is provider to the cluster (common in cloud environments)

External Name
- Reserves a DNS managed by CoreDNS
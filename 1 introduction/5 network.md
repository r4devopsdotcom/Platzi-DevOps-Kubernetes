# Network

- The cluster is in a network of the same segment (i.e. 192.168.0.0/16), the example means that
  the cluster will have 2 bytes of configurable nodes to orchestrate, and allows the communication
  between them.

- All nodes must be connected to each other (without NAT: network access translation). It means that must
  be interconnected through the IP in the same range

- Kube-proxy: Is the component to connect pods and containers

- Pods works at the layer 3 (network) and containers at the layer 4 (transport) of the OSI model

- CNI (container network interface): Allow PODs communicate to each other through IPTABLES that contains
  the map of the physical address to the container IP address
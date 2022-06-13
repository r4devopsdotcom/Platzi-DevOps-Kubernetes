1. Initializes cluster master node:

 kubeadm init --apiserver-advertise-address $(hostname -i) --pod-network-cidr 10.5.0.0/16

    - Paste in the slave nodes
    kubeadm join <network> --token <token> --discovery-token-ca-cert-hash sha256:<token>

    # Requirements to initialize a cluster:
    - Deploy nodes inside the same cluster
    - Deploy networking driver

2. Initialize cluster networking:

 kubectl apply -f https://raw.githubusercontent.com/cloudnativelabs/kube-router/master/daemonset/kubeadm-kuberouter.yaml


3. (Optional) Create an nginx deployment:

 kubectl apply -f https://raw.githubusercontent.com/kubernetes/website/master/content/en/examples/application/nginx-app.yaml
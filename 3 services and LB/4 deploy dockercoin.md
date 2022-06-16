# create cluster
- minikube start --nodes 5 -p bitcoin

# create independent pods
- kubectl create deployment redis --image=redis
- kubectl create deployment hasher --image=santiagortiiz/hasher
- kubectl create deployment rng --image=santiagortiiz/rng

# expose ClusterIP
- kubectl expose deployment redis --port 6379
- kubectl expose deployment rng --port 80
- kubectl expose deployment hasher --port 80

# create dependent pods
- kubectl create deployment webui --image=santiagortiiz/webui           # or webui:v2
- kubectl create deployment worker --image=santiagortiiz/worker

# expose webui node port
- kubectl expose deploy/webui --type=NodePort --port 80

# verify worker activity
- kubectl logs deploy/worker

# If you are in a external host
- kubectl expose deploy/webui --type=NodePort --port=80 --external-ip=127.0.0.1

# open a tunnel to the webui service (did not work)
- minikube -p bitcoin service webui
- kubectl port-forward service/webui 80:8080
- kubectl expose deploy/webui --type=LoadBalancer --port=80
# minikube

minikube profile: used to run and manage multiple minikube instances

### Start the cluster
- minikube start

## Interact with cluster
- minikube kubectl -- get po -A
- minikube dashboard

## Deploy applications

Create a sample deployment and expose it on port 8080
- kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.4
- kubectl expose deployment hello-minikube --type=NodePort --port=8080


Deploy a load balancer
- kubectl create deployment balanced --image=k8s.gcr.io/echoserver:1.4
- kubectl expose deployment balanced --type=LoadBalancer --port=8080

## Access services
- kubectl get services
- minikube service <service_name> | kubectl port-forward service/hello-minikube 7080:8080

## Create routable IP for the load balancer
- minikube tunnel
- kubectl get services balanced

## Cluster management
- minikube start
- minikube pause
- minikube unpause
- minikube stop
- minikube config set memory 16384
- minikube addons list
- minikube start -p aged --kubernetes-version=v1.16.1
- minikube delete --all


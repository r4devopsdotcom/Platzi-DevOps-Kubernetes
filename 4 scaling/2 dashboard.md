# Dashboard setup recipe
- kubectl apply -f .\kubernetes-dashboard.yaml
- minikube ssh -p <profile_name>
- kubectl get svc --all-namespaces | grep dashboard
- curl https://<service_port> -k                            # k: ignore ssl certificates
- kubectl edit service kubernetes-dashboard -n kube-system
- modify spec type of ClusterIP to NodePort
- kubectl get svc -n kube-system                            # Get the PORT of the service
- https://192.168.58.2:31924/

# Enable Dashboards in minikube
- minikube addons enable dashboard
- minikube addons list
- minikube dashboard --url

# Grant dashboard permissions (if needed)
- kubectl apply -f grant-admin-to-dashboard.yaml

# Scaling
- Deployments/name -> set the desired replicas
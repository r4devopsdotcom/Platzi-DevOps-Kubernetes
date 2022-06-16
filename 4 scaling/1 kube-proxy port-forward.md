# Use cases
- remote access to a specific service in the cluster

# kubectl proxy
- foreground proxy running in user space
- grant access to the kubernetes-api (authenticated)

**Allow access to the kubernetes-api with authentication**
- kubectl proxy
- kubectl proxy &               # send to background
- curl http://127.0.0.1:8001

# kubectl port-forward
- grant access to any service exposed in the cluster


- kubectl get svc
```
NAME         TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
hasher       ClusterIP      10.110.227.166   <none>        80/TCP         156m
kubernetes   ClusterIP      10.96.0.1        <none>        443/TCP        3h10m
redis        ClusterIP      10.96.164.16     <none>        6379/TCP       156m
rng          ClusterIP      10.108.6.142     <none>        80/TCP         156m
webui        LoadBalancer   10.97.189.171    <pending>     80:30795/TCP   15m
```

If you want to access to a service from a remote host without expose the service port,
the best way is using port-forward:
- kubectl port-forward service/redis <local_port>:<service_port>
- kubectl port-forward service/redis <local_port>:<service_port> &
- kubectl port-forward service/redis 10000:6379
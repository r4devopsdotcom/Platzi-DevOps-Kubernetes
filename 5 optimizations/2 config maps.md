# Configuration methods
- CMD args
- Environment variables
- **config maps**: Store config files and key value pairs

# create config Maps
- kubectl create configmap <config_name> --from-file=<file.cfg>
- kubectl create configmap haproxy --from-file=haproxy.cfg
- kubectl get configmap haproxy -o yaml

# create a pod that use the created config map
- kubectl apply -f haproxy.yaml
- kubectl get pod haproxy

# test that the pod use the specified config
- minikube sh -p <profile_name>
- curl <pod_IP>
```
curl 10.244.1.3
<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
<TITLE>301 Moved</TITLE></HEAD><BODY>
<H1>301 Moved</H1>
The document has moved
<A HREF="http://www.google.com/">here</A>.
</BODY></HTML>
```

# other way to create config map
- kubectl create configmap <config_name> --from-literal=<key>=<value>
- kubectl create configmap registry --from-literal=http.addr=0.0.0.0:80

# create a pod that use the created config map
- kubectl apply -f registry.yaml        # registry.yaml takes an ENV variable from config map
- kubectl get pod registry

# test that the pod use the specified config
- minikube sh -p <profile_name>
- curl <pod_IP>:80/v2/_catalog
```
curl 10.244.4.5:80/v2/_catalog
{"repositories":[]}
```
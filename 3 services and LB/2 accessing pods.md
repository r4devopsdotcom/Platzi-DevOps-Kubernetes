# create cluster
- minikube start --nodes 3 -p epam

# create pods
- kubectl create deployment httpenv --image jpetazzo/httpenv
- kubectl scale deployment httpenv --replicas 3

# hit one pod
- kubectl get pods -o wide

```
NAME                      READY   STATUS    RESTARTS   AGE     IP           NODE       NOMINATED NODE   READINESS GATES
httpenv-858f759c7-bgwcr   1/1     Running   0          9m17s   10.244.2.2   epam-m03   <none>           <none>
httpenv-858f759c7-lzmj9   1/1     Running   0          48s     10.244.0.3   epam       <none>           <none>
httpenv-858f759c7-mn9l7   1/1     Running   0          48s     10.244.1.2   epam-m02   <none>           <none>
```

- minikube ssh -p epam
- curl http://10.244.2.2:8888
- curl http://10.244.2.2:8888 | jq ""                   # sudo apt-get install jq -y

```
{"HOME":"/root","HOSTNAME":"httpenv-858f759c7-bgwcr","KUBERNETES_PORT":"tcp://10.96.0.1:443","KUBERNETES_PORT_443_TCP":"tcp://10.96.0.1:443","KUBERNETES_PORT_443_TCP_ADDR":"10.96.0.1","KUBERNETES_PORT_443_TCP_PORT":"443","KUBERNETES_PORT_443_TCP_PROTO":"tcp","KUBERNETES_SERVICE_HOST":"10.96.0.1","KUBERNETES_SERVICE_PORT":"443","KUBERNETES_SERVICE_PORT_HTTPS":"443","PATH":"/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"}
```

# create service of type ClusterIP
- kubectl expose deployment httpenv --port 8888         (default type: ClusterIP)

# hit the service instead of a particular pod
- kubectl get service | svc

```
NAME         TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
httpenv      ClusterIP   10.103.145.7   <none>        8888/TCP   71s
kubernetes   ClusterIP   10.96.0.1      <none>        443/TCP    41m
```

- minikube ssh -p epam
- curl http://10.103.145.7:8888

```
{"HOME":"/root","HOSTNAME":"httpenv-858f759c7-bgwcr","KUBERNETES_PORT":"tcp://10.96.0.1:443","KUBERNETES_PORT_443_TCP":"tcp://10.96.0.1:443","KUBERNETES_PORT_443_TCP_ADDR":"10.96.0.1","KUBERNETES_PORT_443_TCP_PORT":"443","KUBERNETES_PORT_443_TCP_PROTO":"tcp","KUBERNETES_SERVICE_HOST":"10.96.0.1","KUBERNETES_SERVICE_PORT":"443","KUBERNETES_SERVICE_PORT_HTTPS":"443","PATH":"/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"}
```

# bash script to hit 10 times the service
for i in $(seq 10); do curl -s http://10.103.145.7:8888; done
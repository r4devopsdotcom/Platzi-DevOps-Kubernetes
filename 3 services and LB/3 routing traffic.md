> This file has a brief explanation about how kubernetes routes the traffic

kubectl get service
```
NAME         TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
httpenv      ClusterIP   10.103.145.7   <none>        8888/TCP   22m
kubernetes   ClusterIP   10.96.0.1      <none>        443/TCP    61m
```

# service IP: 10.103.145.7

View cluster IP table rules to see how kubernetes routes traffic
- sudo iptables -t nat -L OUTPUT
  - t nat: table nat
  - L OUTPUT: list output rules

```
Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination
KUBE-SERVICES  all  --  anywhere             anywhere             /* kubernetes service portals */
DOCKER_OUTPUT  all  --  anywhere             host.minikube.internal
DOCKER     all  --  anywhere            !127.0.0.0/8          ADDRTYPE match dst-type LOCAL
```

- sudo iptables -t nat -nL KUBE-SERVICES
  - nL: do not list names

```
Chain KUBE-SERVICES (2 references)
target     prot opt source               destination
KUBE-SVC-ERIFXISQEP7F7OF4  tcp  --  0.0.0.0/0            10.96.0.10           /* kube-system/kube-dns:dns-tcp cluster IP */ tcp dpt:53
KUBE-SVC-JD5MR3NA4I4DYORP  tcp  --  0.0.0.0/0            10.96.0.10           /* kube-system/kube-dns:metrics cluster IP */ tcp dpt:9153
KUBE-SVC-TCOU7JCQXEZGVUNU  udp  --  0.0.0.0/0            10.96.0.10           /* kube-system/kube-dns:dns cluster IP */ udp dpt:53
KUBE-SVC-6ZVPY37LGINYSSPR  tcp  --  0.0.0.0/0            10.103.145.7         /* default/httpenv cluster IP */ tcp dpt:8888
KUBE-SVC-NPX46M4PTMTKRN6Y  tcp  --  0.0.0.0/0            10.96.0.1            /* default/kubernetes:https cluster IP */ tcp dpt:443
KUBE-NODEPORTS  all  --  0.0.0.0/0            0.0.0.0/0            /* kubernetes service nodeports; NOTE: this must be the last rule in this chain */ ADDRTYPE match dst-type LOCAL
```
# Notice that the rule for the IP of the service: 10.103.145.7: KUBE-SVC-6ZVPY37LGINYSSPR

# Check the description of the rule of the cluster: KUBE-SVC-6ZVPY37LGINYSSPR
- sudo iptables -t nat -nL KUBE-SVC-6ZVPY37LGINYSSPR

```
Chain KUBE-SVC-6ZVPY37LGINYSSPR (1 references)
target     prot opt source               destination
KUBE-MARK-MASQ  tcp  -- !10.244.0.0/16        10.103.145.7         /* default/httpenv cluster IP */ tcp dpt:8888
KUBE-SEP-HX6V2UBOKTXACLGF  all  --  0.0.0.0/0            0.0.0.0/0            /* default/httpenv */ statistic mode random probability 0.33333333349
KUBE-SEP-67XNFGQDJ4EZR42C  all  --  0.0.0.0/0            0.0.0.0/0            /* default/httpenv */ statistic mode random probability 0.50000000000
KUBE-SEP-2BAZO7KUFC5W4VUP  all  --  0.0.0.0/0            0.0.0.0/0            /* default/httpenv */
```
**Notice that it routes using an statistic method baised on output packages**


- sudo iptables -t nat -nL KUBE-SEP-HX6V2UBOKTXACLGF

```
Chain KUBE-SEP-HX6V2UBOKTXACLGF (1 references)
target     prot opt source               destination
KUBE-MARK-MASQ  all  --  10.244.0.3           0.0.0.0/0            /* default/httpenv */
DNAT       tcp  --  0.0.0.0/0            0.0.0.0/0            /* default/httpenv */ tcp to:10.244.0.3:8888
```
**If you look at the destination of the target (10.244.0.3:8888) is the IP of the node:**

- kubectl get pods -o wide
```
NAME                      READY   STATUS    RESTARTS   AGE   IP           NODE       NOMINATED NODE   READINESS GATES
httpenv-858f759c7-bgwcr   1/1     Running   0          49m   10.244.2.2   epam-m03   <none>           <none>
httpenv-858f759c7-lzmj9   1/1     Running   0          41m   10.244.0.3   epam       <none>           <none>
httpenv-858f759c7-mn9l7   1/1     Running   0          41m   10.244.1.2   epam-m02   <none>           <none>
```

# Endpoints (IP addresses of a service)
- kubectl describe service httpenv
```
Name:              httpenv
Namespace:         default
Labels:            app=httpenv
Annotations:       <none>
Selector:          app=httpenv
Type:              ClusterIP
IP Family Policy:  SingleStack
IP Families:       IPv4
IP:                10.103.145.7
IPs:               10.103.145.7
Port:              <unset>  8888/TCP
TargetPort:        8888/TCP
Endpoints:         10.244.0.3:8888,10.244.1.2:8888,10.244.2.2:8888
Session Affinity:  None
Events:            <none>
```

- kubectl describe endpoints httpenv
```
Name:         httpenv
Namespace:    default
Labels:       app=httpenv
Annotations:  endpoints.kubernetes.io/last-change-trigger-time: 2022-06-13T22:26:59Z
Subsets:
  Addresses:          10.244.0.3,10.244.1.2,10.244.2.2
  NotReadyAddresses:  <none>
  Ports:
    Name     Port  Protocol
    ----     ----  --------
    <unset>  8888  TCP

Events:  <none>
```

- kubectl get endpoints httpenv -o yaml
```
apiVersion: v1
kind: Endpoints
metadata:
  annotations:
    endpoints.kubernetes.io/last-change-trigger-time: "2022-06-13T22:26:59Z"
  creationTimestamp: "2022-06-13T22:26:59Z"
  labels:
    app: httpenv
  name: httpenv
  namespace: default
  resourceVersion: "2770"
  uid: 8a941a6f-f792-4ef3-aa0e-7ae880469bee
subsets:
- addresses:
  - ip: 10.244.0.3
    nodeName: epam
    targetRef:
      kind: Pod
      name: httpenv-858f759c7-lzmj9
      namespace: default
      resourceVersion: "2191"
      uid: c4d2a2f2-98f3-4156-be6a-6f53e3c67da4
  - ip: 10.244.1.2
    nodeName: epam-m02
    targetRef:
      kind: Pod
      name: httpenv-858f759c7-mn9l7
      namespace: default
      resourceVersion: "2194"
      uid: daf09ef1-b3de-477e-8cfc-6df9f2f2d597
  - ip: 10.244.2.2
    nodeName: epam-m03
    targetRef:
      kind: Pod
      name: httpenv-858f759c7-bgwcr
      namespace: default
      resourceVersion: "1700"
      uid: 138d2a96-c7fd-4644-90c6-ccdc815875af
  ports:
  - port: 8888
    protocol: TCP

```
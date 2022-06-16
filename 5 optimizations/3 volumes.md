# Volumes for temporal data
Shares information between:
- pods
- containers inside a pod
- pods and host

# Storage mechanisms
- local volumes
- Cloud volumes like AWS/EBS
- NFS
- others

# create a pod with 2 containers that share the same volume
- kubectl apply -f nginx-with-volume.yaml
- kubectl describe pod <pod_name>                   # container1, container2 = nginx, git
- kubectl get pods -o wide                          # get the IP of the pod
- minikube ssh -p <profile_name>
- curl <nginx_pod_IP>
```
curl 10.244.3.4
<!DOCTYPE html>

<html>
<head>
  <meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
  <title>Spoon-Knife</title>
  <LINK href="styles.css" rel="stylesheet" type="text/css">
</head>

<body>

<img src="forkit.gif" id="octocat" alt="" />

<!-- Feel free to change this text here -->
<p>
  Fork me? Fork you, @octocat!
</p>

</body>
</html>
```

# Volumes lifecycle

**emptyDir (OS file system)**
- The volume is created the pod is initialized
- The volume is effective when the first container accesses the volume.

**cloud storage**
- The volume is created when it is defined in the orchestrator

**IMPORTANT**
- The volume is destroyed when the pod is removed
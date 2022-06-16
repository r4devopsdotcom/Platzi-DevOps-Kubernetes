# Healthchecks
Mechanism to check the health state of the app and decide
if it is necessary to redirect the traffic restart any service
or take any other action.

# Types
- Liveness:
  - unrecoverable errors
  - Mechanism: executes a health check until a failure threshold is reached, then try to restart the pod
    until another threshold is reached; if the problem persists, then it stop the retries.
- Readiness:
  - Mechanism: removes the pod until it is ready to receive traffic

**Each type could be of another type, let's call it "sub-type"**
# Sub types:
- http
- tcp
- custom exec (must exists inside the container)

# Create healthcheck
- kubectl edit deploy/redis
**liveness probe**
```
...
spec:
    containers:
    livenessProbe:
        exec:
            command: ["redis-cli", "ping"]
```
- kubectl get pods  (new pod has been created)
- kubectl describe pod <new_pod>                    # Check Liveness rule

# Set nonexistent command
If the command does not exists, it will terminate it and create a new one.
This mechanism is repeated N times. You can see the history of the pod using:
- kubectl describe pod <pod_name-uuid>

**(Verify if the command exists, example with redis pod)**
- kubectl exec <pod_name-uuid> -ti bash
- redis-cli asds
- echo $?
- response: 0               (is also healthy)
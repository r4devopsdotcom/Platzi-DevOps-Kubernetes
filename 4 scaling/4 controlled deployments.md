# Rolling update strategy

# Default strategy

**Get default strategy of the deployments**
(install jq before in linux: sudo apt-get install jq -y)
- kubectl get deploy -o json | jq ".items[] | {name:.metadata.name} + .spec.strategy.rollingUpdate"
```
{
    "name": "deployment_name",
    "maxSurge": "25%",
    "maxUnavailable": "25%"
}
```

maxSurge:
    Indicates how many pods could be in a "creating" state at the same moment.
maxUnavailable:
    max number/percentage of pods that could be unavailable at certain time.
    i.e 25%: If there are 100 pods, 75 must be available.

# Rolling update
- kubectl get deployment -o wide -w                             # (real time tracking)
- kubectl set image deploy <deploy_name> <image_name>=<image_source>:<tag>
- kubectl set image deploy worker worker=dockercoins/worker:v0.2

# Rolling update with errors
- kubectl set image deploy worker worker=dockercoins/worker:v0.3 # (0.3 does not exists)

**At this stage, the cluster will have less pods than expected, let's see how to revert it**
- kubectl roullout undo deploy worker

# Change deploy strategy
- kubectl edit deploy <deploy_name>
- kubectl edit deploy worker
  - Update for example: maxSurge=1, maxUnavailable=0
- kubectl rollout status deployment worker
- kubectl get deploy -o json | jq ".items[] | {name:.metadata.name} + .spec.strategy.rollingUpdate"
```
{
    "name": "deployment_name",
    "maxSurge": 1,
    "maxUnavailable": 0
}
```
# RBAC (Role based access control)
- rolebinding: links a role to a user
- cluster-scope permissions: define permissions at cluster level (not only namespaces)
- A pod could be linked to a token
  - The token is stored in /var/run/secrets

# create service account
- kubectl create sa <name>
- kubectl create sa viewer
- kubectl get sa

# create rolebinding
- kubectl create rolebinding viewercanview --clusterrole=view --serviceaccount=default:viewer
Note:
    - If the specified clusterrole does not exists, the user that use the rolebinding won't have access to anythig until the clusterrole will be created

# link the created service account to a new pod
**eyepod: get available pods**
- kubectl run eyepod --rm -ti --restart=Never --serviceaccount=viewer --image alpine
- apk add --no-cache curl
- download kubernetes for the pod [link](https://storage.googleapis.com/kubernetes-release/release/v0.0.0/bin/linux/amd64/kubectl). Tip: use -LO flag of curl.
- ./kubectl create deployment <name> --image nginx
  - Forbidden: User "system:serviceaccount:default:viewer" cannot create resource deployment

# get user permissions
- kubectl auth can-i list nodes
- kubectl auth can-i create pods
- kubectl auth can-i list nodes --as kube-admin

# get rolebinginds
- kubectl get clusterrolebindings -o yaml
- kubectl get clusterrolebindings -o yaml | grep -e kubernetes-admin -e system:masters
- kubectl describe clusterrolebinding cluster-admin
```
Name:         cluster-admin
Labels:       kubernetes.io/bootstrapping=rbac-defaults
Annotations:  rbac.authorization.kubernetes.io/autoupdate: true
Role:
  Kind:  ClusterRole
  Name:  cluster-admin
Subjects:
  Kind   Name            Namespace
  ----   ----            ---------
  Group  system:masters
```
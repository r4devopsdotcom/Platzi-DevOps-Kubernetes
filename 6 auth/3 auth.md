# Authorization
- On each request, kubernetes-api try to authenticate it using the config in the .kube file
- By default the user is authenticated as admin user with the TLS files in the .kube file
- If the user could not be authenticated is treated as an "anonymous user" without any priviledges

# Authentication methods
- TLS
- Bearer tokens
- Basic Auth (user, password)
- Proxy Auth (3rd party systems)

# Hit the kubernetes api without credentials
- kubectl -k curl https://<kubernetes-api-IP>
```
docker@minikube:~$ curl https://10.96.0.1
curl https://10.96.0.1
{
  "kind": "Status",
  "apiVersion": "v1",
  "metadata": {},
  "status": "Failure",
  "message": "forbidden: User \"system:anonymous\" cannot get path \"/\"",
  "reason": "Forbidden",
  "details": {},
  "code": 403
```

# Verify the credentials used by the command "kubectl"
- kubectl config view --raw -o json
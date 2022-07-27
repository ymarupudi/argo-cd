# 1. Install ArgoCD
```console
kubectl create namespace argocd
```
```console
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
```console
kubectl get all -n argocd
```
# 2. Get the encoded password
```console
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}"
```
Note the output, looks like `aGhqMW1kd1ZxWXZVOXRtYQ==`

# 3. Decode password
```console
echo <step-2-output> | base64 -d
```
Note the output, looks like `hhj1mdwVqYvU9tma`

# 4. Change service type from `ClusterIP` to `NodePort`
```console
kubectl -n argocd edit svc argocd-server
```
<b>Note:</b> Change service of type: `ClusterIP` to type: `NodePort`

# 5. Access ArgoCD Login Page
```console
kubectl -n argocd get svc
```
Note the exposed ports, looks like `80:30021` , `443:31213`
```console
kubectl get nodes -o wide
```
Note the EXTERNAL-IP of node, looks like `54.212.132.136`

---
Browse `EXTERNAL-IP:port`, like `54.212.132.136:30021` or `54.212.132.136:31213`
# 6. Login to ArgoCD Server

<b>Default Username:</b> `admin`

<b>Password:</b> `<step-3-output>`

# 7. Download ArgoCD CLI
https://github.com/argoproj/argo-cd/releases
https://github.com/argoproj/argo-cd/releases/download/v2.3.3/argocd-windows-amd64.exe

# 8. Login to ArgoCD from CLI
```console
argocd login <EXTERNAL-IP:port> --insecure --username admin --password <step-3-output>
```
# 9. Update Password
```console
argocd account update-password --current-password <step-3-output> --new-password <set-your-own-password>
```
# 10. Register a cluster to deploy apps
```console
argocd cluster add <give-a-name-to-cluster> --name <kubernetes-cluster-name>
```
```console
argocd app create <enter-your-app-name> --repo <enter-your-repository-URL> --path <folder-path-in-which-manifests-are-present> --dest-namespace default --dest-server https://kubernetes.default.svc
```
```console
argocd app sync <enter-your-app-name>
```
```console
argocd app get <enter-your-app-name>
```
```console
argocd app resources <enter-your-app-name>
```
```console
argocd app delete <enter-your-app-name>
```
---
# Commands
```console
argocd cluster list
```
```console
argocd proj list
```
```console
argocd repo list
```
```console
argocd app list
```
```console
argocd account get-user-info
```
```console
argocd logout <EXTERNAL-IP:port>
```
# Links
* Config repo: https://github.com/nanda259/argocd.git
* Docker repo: https://hub.docker.com/repository/docker/yoga199/argocd
* Install ArgoCD: https://argo-cd.readthedocs.io/en/stable/getting_started/#1-install-argo-cd
* Login to ArgoCD: https://argo-cd.readthedocs.io/en/stable/getting_started/#4-login-using-the-cli
* ArgoCD Configuration: https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/

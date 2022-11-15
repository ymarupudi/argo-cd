# Download argo-cd CLI
Click [here](https://github.com/argoproj/argo-cd/releases/download/v2.5.2/argocd-windows-amd64.exe) to download argo-cd CLI. Click [here](https://github.com/argoproj/argo-cd/releases) for the latest argo-cd release

Move `argo-cd` application to the folder of your choice and add that folder to the system file path to make it easily accessible from command line.
## 1. Install argo-cd
```console
helm repo add argo https://argoproj.github.io/argo-helm
```
```console
helm repo update
```
```console
helm install argocd --create-namespace --namespace argocd-system argo/argo-cd
```
```console
kubectl get pods -n argocd-system
```
## 2. Retrieve the password with below command and note it
```console
kubectl -n argocd-system get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```
## 3. Access the argo-cd
### Approach 1
Port Forwarding
```console
kubectl port-forward service/argocd-server -n argocd-system 8080:443
```
To login, browse `127.0.0.1:8080`

<b>Default Username:</b> `admin`

<b>Password:</b> `<enter-step-2-output>`

To login using the argo-cd CLI
```console
argocd login localhost:8080 --insecure --username admin --password <enter-step-2-output>
```
### Approach 2
Change the argocd-server service type: `ClusterIP` to type: `NodePort`
```console
kubectl edit service/argocd-server -n argocd-system
```
Run below command and note the exposed ports, looks like `80:30021` , `443:31213`
```console
kubectl -n argocd get svc
```
Run below command and note the `EXTERNAL-IP` of node, looks like `54.212.132.136`
```console
kubectl get nodes -o wide
```
To login, browse `EXTERNAL-IP:port`, example... `54.212.132.136:30021` or `54.212.132.136:31213`

<b>Default Username:</b> `admin`

<b>Password:</b> `<enter-step-2-output>`

To login using the argo-cd CLI
```console
argocd login <EXTERNAL-IP:port> --insecure --username admin --password <enter-step-2-output>
```
## 4. To update argo-cd password
```console
argocd account update-password --current-password <step-2-output> --new-password <set-your-own-password>
```
## 5. Deploy app
### i) Create an application
```console
argocd app create <enter-your-app-name> --repo <enter-your-repository-URL> --path <folder-path-in-which-manifests-are-present> --dest-namespace default --dest-server https://kubernetes.default.svc
```
### ii) List all the applications
```console
argocd app list
```
### iii) Sync an application to its target state
```console
argocd app sync <enter-your-app-name>
```
### iv) Get application details
```console
argocd app get <enter-your-app-name>
```
### v) List resource of application
```console
argocd app resources <enter-your-app-name>
```
### vi) Delete an application
```console
argocd app delete <enter-your-app-name>
```
### vii) Delete resource in an application
```console
argocd app delete-resource <enter-your-app-name>
```
### viii) List projects
```console
argocd proj list
```
### ix) List configured repositories
```console
argocd repo list
```
### x) Get user info
```console
argocd account get-user-info
```
### xi) Log out from Argo CD
```console
argocd logout <EXTERNAL-IP:port>
```
## 6. Register an external cluster to deploy apps
### i) List all clusters contexts in your current kubeconfig
```console
kubectl config get-contexts
```
   <b>Note:</b> Store `NAME` list

### ii) Add a target cluster configuration to ArgoCD. The context must exist in your kubectl config:
```console
argocd cluster add <NAME>
```
or
```console
argocd cluster add <NAME> --name <name-your-cluster-in-argocd-if-required>
```
### iii) List all known clusters
```console
argocd cluster list
```
### iv) Remove a target cluster context from ArgoCD
```console
argocd cluster rm <NAME>
```
# Links
* Config repo: https://github.com/nanda259/argocd.git
* Docker repo: https://hub.docker.com/repository/docker/yoga199/argocd
* Install ArgoCD: https://argo-cd.readthedocs.io/en/stable/getting_started/#1-install-argo-cd
* Login to ArgoCD: https://argo-cd.readthedocs.io/en/stable/getting_started/#4-login-using-the-cli
* ArgoCD Configuration: https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/
* Command Reference: https://argo-cd.readthedocs.io/en/stable/user-guide/commands/argocd/

# Commands

kubectl create namespace argocd

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

kubectl get all -n argocd

# Enter below command to get the encoded password

kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}"

Output: aGhqMW1kd1ZxWXZVOXRtYQ==

# Enter below command to decode password

echo aGhqMW1kd1ZxWXZVOXRtYQ== | base64 -d

Output: hhj1mdwVqYvU9tma

# Edit service to change type to NodePort

kubectl -n argocd edit svc argocd-server

Change service type as NodePort (type: NodePort)

# Follow below steps to access ArgoCD Login Page

kubectl -n argocd get svc

Note the exposed ports: (80:30021 , 443:31213)

kubectl get nodes -o wide

Note the EXTERNAL-IP of node: 54.212.132.136

Browse 54.212.132.136:30021 or 54.212.132.136:31213

Default Username: admin

Password: hhj1mdwVqYvU9tma

# Download ArgoCD CLI

https://github.com/argoproj/argo-cd/releases

https://github.com/argoproj/argo-cd/releases/download/v2.3.3/argocd-windows-amd64.exe


# Login to ArgoCD

argocd login 54.212.132.136:31213 --insecure --username admin --password hhj1mdwVqYvU9tma

# Update Password

argocd account update-password --current-password abcde@123 --new-password abcde@456

argocd cluster list

argocd proj list

argocd repo list

argocd app list

argocd account get-user-info

argocd logout 54.212.132.136:31213

# Register a cluster to deploy apps in it.

argocd cluster add

argocd app create argocd-demo --repo https://github.com/nanda259/argocd --path app-config --dest-namespace default --dest-server https://kubernetes.default.svc

argocd app sync argocd-demo

argocd app get argocd-demo

argocd app resources argocd-demo

argocd app delete argocd-demo


# Links

* Config repo: https://github.com/nanda259/argocd.git
* Docker repo: https://hub.docker.com/repository/docker/yoga199/argocd
* Install ArgoCD: https://argo-cd.readthedocs.io/en/stable/getting_started/#1-install-argo-cd
* Login to ArgoCD: https://argo-cd.readthedocs.io/en/stable/getting_started/#4-login-using-the-cli
* ArgoCD Configuration: https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/

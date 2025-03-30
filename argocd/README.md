# ArgoCD
## Links
- https://argoproj.github.io/cd/
- https://argo-cd.readthedocs.io/en/stable/
- https://github.com/argoproj/argo-helm/tree/master/charts/argo-cd
- https://artifacthub.io/packages/helm/argo/argo-cd
- https://github.com/argoproj/argo-helm/blob/main/charts/argo-cd/values.yaml
## CLI
```bash
cd ~/bin
# https://github.com/argoproj/argo-cd/releases
VERSION=v2.12.6
VERSION=v2.14.8
curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/download/$VERSION/argocd-linux-amd64
mv argocd-linux-amd64 argocd
chmod +x argocd
```
## Usage
```bash
colima start

# https://kind.sigs.k8s.io/
kind create cluster --name argocd
kubectl cluster-info --context kind-argocd

# https://artifacthub.io/packages/helm/argo/argo-cd
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update
VERSION=7.6.12
VERSION=7.8.15
helm install argocd argo/argo-cd --version $VERSION -f values.yaml --wait
helm upgrade argocd argo/argo-cd --version $VERSION -f values.yaml --wait

# admin password
ADMIN_PASS=$(kubectl get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d)
echo $ADMIN_PASS

# add local test user
# accounts.test: apiKey, login
kubectl edit configmap argocd-cm
argocd login localhost:8080 --username admin --password $ADMIN_PASS --insecure
argocd account update-password --account test --new-password test1234

kubectl port-forward service/argocd-argocd-server -n default 8080:443
http://localhost:8080

# clean up
kind delete cluster --name argocd
colima stop
```
## Local user
References:
- https://devopscube.com/create-a-new-account-in-argo-cd/
```bash
argocd login localhost:8080 --username admin --password ADMIN_PASS --grpc-web
argocd account list

kubectl -n argocd edit configmap argocd-cm
# data:
#   accounts.user1: login
argocd account list

kubectl -n argocd edit configmap argocd-rbac-cm
# policy.csv: |
#   p, role:readonly, applications, get, *, allow
#   g, user1, role:readonly

argocd account update-password --account user1 --current-password ADMIN_PASS --new-password NEW_USER_PASS
```

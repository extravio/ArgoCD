# Argo CD

## Installation

[Installing ArgoCD on Minikube and deploying a test application](https://medium.com/@mehmetodabashi/installing-argocd-on-minikube-and-deploying-a-test-application-caa68ec55fbf)

```
kubectl create ns argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/v2.13.1/manifests/install.yaml
kubectl get all -n argocd
kubectl port-forward svc/argocd-server -n argocd 8080:443
# Get admin secret
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```

### Install ArgoCD Client

[Installation](https://argo-cd.readthedocs.io/en/stable/cli_installation/)

``` 
# Get password
argocd admin initial-password -n argocd
argocd login localhost:8080
```

## Example App

[ArgoCD Example Apps](https://github.com/argoproj/argocd-example-apps.git)

```
kubectl config set-context --current --namespace=argocd
kubectl create ns argocd-ex1
argocd app create guestbook --repo https://github.com/argoproj/argocd-example-apps.git --path guestbook --dest-server https://kubernetes.default.svc --dest-namespace argocd-ex1
# Delete App
argocd app delete guestbook 
```
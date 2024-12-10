# Argo CD - Minikube

## Installation

```
wget https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
chmod +x minikube-linux-amd64
./minikube-linux-amd64 start
```

## Helm + Kustomize

https://trstringer.com/helm-kustomize/

Option 1:
```
helm template ./helm-kustomize --set image.tag=0.1.0 > resources.yaml
kubectl kustomize > resources-kustomized.yaml
```

Option 2:
```
helm template ./helm-kustomize --set image.tag=0.1.0 --post-renderer ./kustomize.sh --debug > resources.kustomized.yaml

helm upgrade hk ./helm-kustomize --set image.tag=0.1.0 --install --post-renderer ./kustomize.sh
```

## Deploy Helm with ArgoCD

```
kubectl config set-context --current --namespace=argocd
kubectl create ns my-app
argocd app create helm --repo https://github.com/extravio/ArgoCD.git --path kustomize/helm --dest-server https://kubernetes.default.svc --dest-namespace my-app
# Get App status
argocd app get helm
# Sync App
argocd app get helm
# Delete App
argocd app delete guestbook 
```


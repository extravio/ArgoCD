# Helm + Kustomize

## Helm

```
helm template ./helm-kustomize-tag --set image.tag=0.1.0 > resources.yaml

helm template . --post-renderer ./kustomize.sh --debug > resources.kustomized.yaml
```

## Deploy Helm with ArgoCD

```
# App creation (path: helm)
argocd app create helm --repo https://github.com/extravio/ArgoCD.git --path kustomize/helm --dest-server https://kubernetes.default.svc --dest-namespace my-app
# Get App status
argocd app get helm
# Edit App (path: helm-kustomize-tag)
argocd app edit helm --path kustomize/helm-kustomize-app

# Sync App
argocd app get helm
# Delete App
argocd app delete guestbook 
```


# Helm + Kustomize - Hello World

[Hello World chart](https://github.com/helm/examples/tree/main/charts/hello-world)

## Deploy Helm with ArgoCD

```
# App creation (path: helm-enabled-kustomize)
argocd app create helm --repo https://github.com/extravio/ArgoCD.git --path kustomize/helm-hello-world --revision dev --dest-server https://kubernetes.default.svc --dest-namespace my-app --config-management-plugin kustomize-helm-enabled


# App creation (path: helm)
argocd app create helm --repo https://github.com/extravio/ArgoCD.git --path kustomize/helm --dest-server https://kubernetes.default.svc --dest-namespace my-app
# Get App status
argocd app get helm
# Update Target Revision (master -> dev)
argocd app patch helm --patch '{"spec": { "source": { "targetRevision": "dev" } }}' --type merge
# Edit App (path: helm-kustomize-tag)
argocd app patch helm --patch='[{"op": "replace", "path": "/spec/source/path", "value": "kustomize/helm-hello-world"}]' --type json

# Sync App
argocd app get helm
# Delete App
argocd app delete guestbook 
```

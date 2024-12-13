# Helm + Kustomize


Example [Helm + Kustomize](https://github.com/argoproj/argocd-example-apps/tree/master/plugins/kustomized-helm)

Issues:

[ArgoCD app with kustomize helm - helm resources deploy to default namespace](https://github.com/argoproj/argocd-example-apps/issues/187)

[Update kustomized-helm as a sidecar plugin](https://github.com/argoproj/argocd-example-apps/issues/177)


[document support for --enable-helm flag for kustomize](https://github.com/argoproj/argo-cd/issues/7835)



Convert 

```
 configManagementPlugins: |
    - name: kustomized-helm
      init:
        command: ["/bin/sh", "-c"]
        args: ["helm dependency build"]
      generate:
        command: [sh, -c]
        args: ["helm template --release-name release-name . > all.yaml && kustomize build"]
```

to a [config file](https://argo-cd.readthedocs.io/en/stable/operator-manual/config-management-plugins/#convert-the-configmap-entry-into-a-config-file)


Customize argocd-cm ConfigMap 
key: `kustomize.buildOptions`
value: `--enable-helm`


## Helm

```
helm template ./helm-kustomize-tag --set image.tag=0.1.0 > resources.yaml

helm template . --post-renderer ./kustomize.sh --debug > resources.kustomized.yaml
```

## Deploy Helm with ArgoCD

```
# App creation (path: helm-enabled-kustomize)
argocd app create helm --repo https://github.com/extravio/ArgoCD.git --path kustomize/helm-enabled-kustomize --revision dev --dest-server https://kubernetes.default.svc --dest-namespace my-app --config-management-plugin kustomized-helm


# App creation (path: helm)
argocd app create helm --repo https://github.com/extravio/ArgoCD.git --path kustomize/helm --dest-server https://kubernetes.default.svc --dest-namespace my-app
# Get App status
argocd app get helm
# Update Target Revision (master -> dev)
argocd app patch helm --patch '{"spec": { "source": { "targetRevision": "dev" } }}' --type merge
# Edit App (path: helm-kustomize-tag)
argocd app patch helm --patch='[{"op": "replace", "path": "/spec/source/path", "value": "kustomize/helm-enabled-kustomize"}]' --type json

# Sync App
argocd app get helm
# Delete App
argocd app delete guestbook 
```

  # Update an application's source path using json patch
  

  # Update an application's repository target revision using merge patch
  argocd app patch myapplication --patch '{"spec": { "source": { "targetRevision": "master" } }}' --type merge


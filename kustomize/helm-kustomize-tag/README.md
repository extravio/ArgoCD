# Helm - Kustomize 

Kustomize tag - local development

## Helm

```
helm template ./helm-kustomize-tag --set image.tag=0.1.0 > resources.yaml

helm template . --post-renderer ./kustomize.sh --debug > resources.kustomized.yaml
```
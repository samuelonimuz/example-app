# example-app

Workload repository — pure Kubernetes manifests, gemanaged via Kustomize.

Deze repo wordt **niet rechtstreeks** gedeployed; ArgoCD pickt hem op via een
`ApplicationSet` definitie in
[openshift-gitops-apps](https://github.com/samuelonimuz/argocd).

## Structuur

```text
base/                       # gemeenschappelijke manifests voor alle clusters
  deployment.yaml
  service.yaml
  kustomization.yaml
overlays/
  sandbox1/                 # per-cluster aanpassingen
    kustomization.yaml
```

## Lokaal testen

```bash
kustomize build overlays/sandbox1
# of met kubectl:
kubectl kustomize overlays/sandbox1
```

## Nieuwe cluster toevoegen

1. Maak `overlays/<cluster-name>/kustomization.yaml` aan.
2. Pas indien nodig `replicas`, hostnames, of resources aan.
3. Zorg dat er een cluster in ArgoCD geregistreerd staat met label
   `name=<cluster-name>` — de `ApplicationSet` in de definitions repo pickt
   hem automatisch op.

# atlas-gitops

GitOps repository for Atlas: the single source of truth for the applications' desired state. Holds
the Argo CD `Application` manifests and per-service values. Argo CD watches this repository and
reconciles the declared state into the cluster.

## Contents

- `apps/<service>/application.yaml` — Argo CD `Application` for each service (multi-source: base
  chart from `atlas-templates`, values from here).
- `apps/<service>/values.yaml` — per-service desired state (replicas, env, image, service).

## Stack

- Argo CD (`Application`)
- Helm values

## Run

```bash
# Apply a service's Application to the cluster (Argo CD reconciles it)
kubectl apply -f apps/hello/application.yaml
argocd app get hello
```

## Test

```bash
# Validate manifests
kubectl apply --dry-run=client -f apps/hello/application.yaml
```

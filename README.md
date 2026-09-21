# atlas-gitops

GitOps repository for Atlas: the single source of truth for the applications' desired state. Holds
the Argo CD `Application` manifests and per-service values. Argo CD watches this repository and
reconciles the declared state into the cluster.

## Contents

- `apps/<service>/application.yaml` — Argo CD `Application` for each service (multi-source: base
  chart from `atlas-templates`, values from here).
- `apps/<service>/values.yaml` — per-service desired state (replicas, env, image, service).
- `bootstrap/root.yaml` — root `Application` (app-of-apps): discovers every `apps/**/application.yaml`
  so a single apply brings all services back from git.

## Stack

- Argo CD (`Application`)
- Helm values

## Run

The root Application (`bootstrap/root.yaml`, applied once at bootstrap) discovers every
`apps/<service>/application.yaml` and reconciles it. To inspect:

```bash
kubectl -n argocd get applications
argocd app get <service>
```

## Test

```bash
# Validate a service's manifest
kubectl apply --dry-run=client --validate=false -f apps/<service>/application.yaml
```

> `--validate=false` is required because the `Application` kind is a CRD; full schema
> validation only works against a cluster where the Argo CD CRDs are installed.

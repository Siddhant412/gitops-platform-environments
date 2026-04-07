# GitOps Platform Environments

This repository owns deployment desired state for services managed by the internal developer platform.

## Layout

- `bootstrap/` contains shared namespaces and Argo CD project definitions
- `bootstrap/root-app.yaml` creates the Argo CD app-of-apps entrypoint
- `apps/` contains per-service Kubernetes manifests and environment overlays
- `argocd/applications/` contains Argo CD `Application` manifests that point at the overlays in this repo

## Flow

1. A service repo builds and publishes an image to GHCR.
2. CI in the service repo updates the matching overlay in this repo with the new image tag.
3. The root Argo CD application watches `argocd/applications/`.
4. Argo CD syncs the changed overlay into Kubernetes.

## Bootstrap

Apply the bootstrap directory once to a cluster that already has Argo CD installed:

```bash
kubectl apply -k bootstrap
```

That creates:

- shared namespaces
- the `platform-dev` Argo CD project
- the `platform-environments-root` app-of-apps entrypoint

After that, newly added files under `argocd/applications/` are discovered automatically by Argo CD.

# GitOps Platform Environments

This repository owns deployment desired state for services managed by the internal developer platform.

## Layout

- `bootstrap/` contains shared namespaces and Argo CD project definitions
- `apps/` contains per-service Kubernetes manifests and environment overlays
- `argocd/applications/` contains Argo CD `Application` manifests that point at the overlays in this repo

## Flow

1. A service repo builds and publishes an image to GHCR.
2. CI in the service repo updates the matching overlay in this repo with the new image tag.
3. Argo CD watches this repo and syncs the changed overlay into Kubernetes.

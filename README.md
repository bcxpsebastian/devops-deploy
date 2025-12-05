dogs — Kubernetes deployment repository

This repository contains a minimal Kubernetes application for the `dogs` image stored in GitHub Container Registry. It's intended to be referenced by Argo CD for automated installation.

What is included
- Kubernetes manifests in `k8s/` (Namespace, Deployment, Service, Ingress, Kustomize)
- An Argo CD `Application` manifest in `argocd/application.yaml` (update `repoURL`)

Quick setup

1. Update image and repo placeholders:
   - Edit `k8s/deployment.yaml` and replace `ghcr.io/<GITHUB_OWNER>/dogs:latest` with your image path.
   - Edit `argocd/application.yaml` and set `spec.source.repoURL` to this repository's git URL.

2. If your image is private, configure an `imagePullSecret` in the `dogs` namespace and reference it in the deployment.

Using Argo CD

- Apply the Argo CD Application to your Argo CD control plane (example assumes `kubectl` is configured for the Argo CD cluster):

```bash
# from a machine with access to the ArgoCD API server / cluster
kubectl apply -f argocd/application.yaml
```

Notes
- The manifests use placeholders (`<GITHUB_OWNER>`, `dogs.example.com`, `https://github.com/<org>/deploy.git`). Replace them with real values before installing.
- The kustomize base is `k8s/` so Argo CD can point to `path: k8s` when creating the Application.

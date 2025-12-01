# K8s Test - ArgoCD & Argo Workflow Demo

A personal Kubernetes lab environment for demonstrating **ArgoCD** and **Argo Workflow** capabilities.

## Purpose

This repository showcases:

- **GitOps with ArgoCD** - Declarative, Git-based application deployment
- **Argo Workflow** - Container-native workflow orchestration (CI/CD pipelines)
- **Kubernetes fundamentals** - Deployments, Services, Ingress, HPA

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Local K8s Cluster                         │
│                      (kubeadm/Vagrant)                       │
│                                                              │
│  ┌─────────────┐    ┌─────────────┐    ┌─────────────┐      │
│  │   ArgoCD    │    │ Argo        │    │   NGINX     │      │
│  │             │    │ Workflow    │    │   Ingress   │      │
│  └──────┬──────┘    └─────────────┘    └─────────────┘      │
│         │                                                    │
│         │ GitOps Sync                                        │
│         ▼                                                    │
│  ┌─────────────────────────────────────────────────────┐    │
│  │              Application Components                  │    │
│  │  ┌──────────┐  ┌──────────┐  ┌───────────────┐      │    │
│  │  │ nginx-web│  │nodejs-k8s│  │ kafka-connect │      │    │
│  │  └──────────┘  └──────────┘  └───────────────┘      │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

## Repository Structure

```
.
├── applications/           # ArgoCD Application manifests
│   ├── argocd.yaml
│   ├── argo-workflow.yaml
│   ├── nginx-web.yaml
│   ├── nodejs-k8s-app.yaml
│   ├── kafka-connect.yaml
│   └── ...
│
└── components/             # Kubernetes manifests for each app
    ├── argocd/
    ├── argo-workflow/
    ├── nginx-web/
    ├── nodejs-k8s/
    ├── kafka-connect/      # Streaming platform example
    └── ...
```

## Limitations

> ⚠️ This is a **demo/learning environment**, not production-ready.

- No environment separation (dev/staging/prod)
- No nested variable folders or Kustomize overlays
- No secrets management (Sealed Secrets, External Secrets, etc.)
- Single-node cluster setup

## Related Projects

- [kraken-demo](https://github.com/leonlaf66/kraken-demo) - Production-grade data platform with Terraform/Spacelift
- [kraken-demo-module](https://github.com/leonlaf66/kraken-demo-module) - Reusable Terraform modules

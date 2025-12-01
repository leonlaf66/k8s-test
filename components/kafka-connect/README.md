# Kafka Connect on Kubernetes

Example deployment of Kafka Connect (S3 Sink) on Kubernetes, managed by ArgoCD.

## Overview

This demonstrates how **MSK Connect workloads can be migrated to EKS** and managed via GitOps.

## Streaming Platform Components

The following components (currently deployed as ECS services or MSK Connect in the [kraken-demo](https://github.com/leonlaf66/kraken-demo-project) project) can all be containerized and deployed to EKS with ArgoCD:

| Component | Current (AWS) | EKS Alternative | Docker Image |
|-----------|---------------|-----------------|--------------|
| **Kafka Connect** | MSK Connect | This example | `confluentinc/cp-kafka-connect` |
| **Schema Registry** | ECS Fargate | K8s Deployment | `confluentinc/cp-schema-registry` |
| **Cruise Control** | ECS Fargate | K8s Deployment | `linkedin/cruise-control` |
| **Prometheus** | ECS Fargate | K8s Deployment | `prom/prometheus` |
| **Alertmanager** | ECS Fargate | K8s Deployment | `prom/alertmanager` |

## Why EKS over MSK Connect / ECS?

| Consideration | MSK Connect / ECS | EKS |
|---------------|-------------------|-----|
| Management | AWS managed | Self-managed |
| Cost | Pay per MCU / vCPU | Shared cluster resources |
| Flexibility | Limited | Full control |
| GitOps | Terraform only | ArgoCD + Helm + Kustomize |
| Portability | AWS only | Multi-cloud |

**Choose MSK Connect / ECS when:** You want AWS-managed, less operational overhead.

**Choose EKS when:** You need GitOps, cost optimization, or multi-cloud portability.

## Files

```
kafka-connect/
├── deployment.yaml   # Kafka Connect worker + ConfigMap
├── service.yaml      # ClusterIP service
└── secret.yaml       # AWS credentials
```

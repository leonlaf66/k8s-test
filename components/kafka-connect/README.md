# Kafka Connect on Kubernetes

Example deployment of Kafka Connect (S3 Sink) on Kubernetes, managed by ArgoCD.

## Overview

This demonstrates how **MSK Connect workloads can be migrated to EKS** and managed via GitOps.

## Streaming Platform Components

The following components (currently deployed as ECS services or MSK Connect in the [kraken-demo](https://github.com/leonlaf66/kraken-demo) project) can all be containerized and deployed to EKS with ArgoCD:

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
├── service.yaml      # ClusterIP service (port 8083)
└── secret.yaml       # AWS credentials (optional)
```

## Usage

```bash
# Deploy via ArgoCD
kubectl apply -f applications/kafka-connect.yaml

# Check status
kubectl get pods -n kafka-connect

# Access REST API
kubectl port-forward svc/kafka-connect 8083:8083 -n kafka-connect
curl http://localhost:8083/connectors
```

## Creating a Connector

```bash
curl -X POST http://localhost:8083/connectors \
  -H "Content-Type: application/json" \
  -d '{
    "name": "s3-sink-example",
    "config": {
      "connector.class": "io.confluent.connect.s3.S3SinkConnector",
      "tasks.max": "1",
      "topics": "your-topic",
      "s3.bucket.name": "your-bucket",
      "s3.region": "us-east-1",
      "flush.size": "1000"
    }
  }'
```

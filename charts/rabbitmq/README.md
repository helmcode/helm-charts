# RabbitMQ Helm Chart

A Helm chart for deploying RabbitMQ using the [RabbitMQ Cluster Operator](https://www.rabbitmq.com/kubernetes/operator/operator-overview).

## Prerequisites

- Kubernetes 1.19+
- Helm 3.0+
- [RabbitMQ Cluster Operator](https://github.com/rabbitmq/cluster-operator) installed in your cluster

### Installing the Operator

```bash
kubectl apply -f https://github.com/rabbitmq/cluster-operator/releases/latest/download/cluster-operator.yml
```

## Installation

```bash
helm install my-rabbitmq oci://ghcr.io/helmcode/helm-charts/rabbitmq
```

## Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `spec.replicas` | Number of RabbitMQ replicas | `3` |
| `spec.image` | RabbitMQ image | `rabbitmq:3.13.7-management` |
| `spec.service.type` | Kubernetes service type | `ClusterIP` |
| `spec.persistence.storageClassName` | Storage class for PVCs | `gp2` |
| `spec.persistence.storage` | Storage size per replica | `10Gi` |

## Example

Custom values file:

```yaml
spec:
  replicas: 5
  image: rabbitmq:3.13.7-management
  service:
    type: LoadBalancer
  persistence:
    storageClassName: standard
    storage: 20Gi
```

Install with custom values:

```bash
helm install my-rabbitmq oci://ghcr.io/helmcode/helm-charts/rabbitmq -f values-custom.yaml
```

## Uninstallation

```bash
helm uninstall my-rabbitmq
```

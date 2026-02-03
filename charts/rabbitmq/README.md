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
| `loadBalancer.enabled` | Enable optional LoadBalancer Service | `false` |
| `loadBalancer.type` | Service type (LoadBalancer or NodePort) | `LoadBalancer` |
| `loadBalancer.annotations` | Cloud provider annotations | `{}` |
| `loadBalancer.loadBalancerSourceRanges` | IP ranges allowed to access the LoadBalancer | `[]` |
| `loadBalancer.ports.amqp.port` | AMQP port | `5672` |
| `loadBalancer.ports.management.port` | Management UI port | `15672` |

### LoadBalancer Configuration

By default, the RabbitMQ Cluster Operator creates a ClusterIP service for internal cluster access. For external access (e.g., connecting from local development via VPN), you can enable an optional LoadBalancer Service.

**Important**: LoadBalancer services may incur cloud provider costs. This feature is disabled by default.

#### Enable LoadBalancer

```yaml
loadBalancer:
  enabled: true
```

#### AWS Configuration (Internal NLB)

For AWS environments with VPN access, configure an internal Network Load Balancer:

```yaml
loadBalancer:
  enabled: true
  annotations:
    service.beta.kubernetes.io/aws-load-balancer-type: "nlb"
    service.beta.kubernetes.io/aws-load-balancer-internal: "true"
    service.beta.kubernetes.io/aws-load-balancer-scheme: "internal"
  loadBalancerSourceRanges:
    - "10.0.0.0/8"  # VPN CIDR block
```

#### GCP Configuration (Internal Load Balancer)

For GCP environments:

```yaml
loadBalancer:
  enabled: true
  annotations:
    cloud.google.com/load-balancer-type: "Internal"
  loadBalancerSourceRanges:
    - "10.0.0.0/8"  # VPN CIDR block
```

#### Azure Configuration (Internal Load Balancer)

For Azure environments:

```yaml
loadBalancer:
  enabled: true
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
  loadBalancerSourceRanges:
    - "10.0.0.0/8"  # VPN CIDR block
```

#### Access Restrictions

Use `loadBalancerSourceRanges` to restrict access to specific IP ranges (e.g., your VPN CIDR blocks):

```yaml
loadBalancer:
  enabled: true
  loadBalancerSourceRanges:
    - "192.168.1.0/24"  # Office network
    - "10.0.0.0/8"      # VPN network
```

#### Exposed Ports

The LoadBalancer Service exposes:
- **5672**: AMQP protocol (client connections)
- **15672**: Management UI (web interface)

To connect from your local machine after enabling the LoadBalancer:

```bash
# Get the LoadBalancer external IP
kubectl get svc <release-name>-rabbitmq-loadbalancer

# Connect using AMQP client
# amqp://<external-ip>:5672

# Access Management UI in browser
# http://<external-ip>:15672
```

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

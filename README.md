# Helm Charts

A collection of Helm charts maintained by [Helmcode](https://github.com/helmcode).

## Available Charts

| Chart | Description |
|-------|-------------|
| [rabbitmq](./charts/rabbitmq) | RabbitMQ using the RabbitMQ Cluster Operator |

## Usage

Charts are published to GitHub Container Registry (GHCR) as OCI artifacts.

```bash
# Pull a chart
helm pull oci://ghcr.io/helmcode/helm-charts/rabbitmq

# Install directly
helm install my-rabbitmq oci://ghcr.io/helmcode/helm-charts/rabbitmq
```

## Prerequisites

Each chart may have specific prerequisites. For example, the `rabbitmq` chart requires the [RabbitMQ Cluster Operator](https://www.rabbitmq.com/kubernetes/operator/operator-overview) to be installed in your cluster.

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## License

Apache License 2.0 - see [LICENSE](./LICENSE) for details.

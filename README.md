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

See [CONTRIBUTING.md](./CONTRIBUTING.md) for detailed instructions on:
- Adding new charts
- Releasing chart updates
- Versioning rules

## License

Apache License 2.0 - see [LICENSE](./LICENSE) for details.

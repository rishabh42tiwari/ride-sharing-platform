# Ride Sharing Platform - Helm Chart

This repository contains the Helm chart for deploying the Ride Sharing Platform microservices application.

## Overview

The Ride Sharing Platform is a distributed microservices-based application that provides ride-hailing functionality. This Helm chart packages all the necessary Kubernetes resources for deploying the platform.

## Architecture

The platform consists of the following microservices:

- **API Gateway**: Entry point for all client requests
- **Trip Service**: Manages ride requests and trip lifecycle
- **Driver Service**: Handles driver management and location tracking
- **Payment Service**: Processes payments and transactions
- **Web**: Frontend application
- **RabbitMQ**: Message broker for event-driven communication
- **Jaeger**: Distributed tracing and observability

## Prerequisites

- Kubernetes cluster (v1.19+)
- Helm 3.x
- kubectl configured to communicate with your cluster

## Installation

### 1. Add the repository (if published to a Helm repository)

```bash
helm repo add ride-sharing <repository-url>
helm repo update
```

### 2. Install the chart

```bash
# Install with default values
helm install ride-sharing-platform .

# Install with custom values
helm install ride-sharing-platform . -f custom-values.yaml

# Install in a specific namespace
helm install ride-sharing-platform . --namespace ride-sharing --create-namespace
```

## Configuration

The following table lists the configurable parameters and their default values. Refer to `values.yaml` for the complete list of configuration options.

### Customizing the Installation

Create a `custom-values.yaml` file to override default values:

```yaml
# Example custom values
apiGateway:
  replicaCount: 3
  image:
    tag: "v1.2.0"

tripService:
  resources:
    limits:
      memory: "512Mi"
      cpu: "500m"
```

Then install with:

```bash
helm install ride-sharing-platform . -f custom-values.yaml
```

## Upgrading

```bash
# Upgrade with new values
helm upgrade ride-sharing-platform . -f custom-values.yaml

# Upgrade with specific version
helm upgrade ride-sharing-platform . --version 1.0.1
```

## Uninstalling

```bash
helm uninstall ride-sharing-platform

# Uninstall from specific namespace
helm uninstall ride-sharing-platform --namespace ride-sharing
```

## Development

### Chart Structure

```
.
├── Chart.yaml          # Chart metadata
├── values.yaml         # Default configuration values
├── charts/             # Chart dependencies
└── templates/          # Kubernetes manifest templates
    ├── api-gateway/
    ├── driver-service/
    ├── payment-service/
    ├── trip-service/
    ├── web/
    ├── rabbitmq/
    └── jaeger/
```

### Testing the Chart

```bash
# Lint the chart
helm lint .

# Dry run to see generated manifests
helm install ride-sharing-platform . --dry-run --debug

# Template the chart
helm template ride-sharing-platform .
```

## Monitoring and Observability

The platform includes Jaeger for distributed tracing. Access the Jaeger UI:

```bash
kubectl port-forward svc/jaeger-query 16686:16686
```

Then navigate to `http://localhost:16686`

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test the chart thoroughly
5. Submit a pull request

## License

[Add your license here]

## Support

For issues and questions, please open an issue in the GitHub repository.

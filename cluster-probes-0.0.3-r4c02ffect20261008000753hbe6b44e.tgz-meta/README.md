[![CircleCI](https://dl.circleci.com/status-badge/img/gh/giantswarm/cluster-probes/tree/main.svg?style=svg)](https://dl.circleci.com/status-badge/redirect/gh/giantswarm/cluster-probes/tree/main)
[![OpenSSF Scorecard](https://api.securityscorecards.dev/projects/github.com/giantswarm/cluster-probes/badge)](https://securityscorecards.dev/viewer/?uri=github.com/giantswarm/cluster-probes)

[Guide about how to manage an app on Giant Swarm](https://handbook.giantswarm.io/docs/dev-and-releng/app-developer-processes/adding_app_to_appcatalog/)

# cluster-probes

A GiantSwarm Helm chart for monitoring workload cluster API server availability from the management cluster.

## What is cluster-probes?

`cluster-probes` is a Helm chart that deploys Kubernetes `ServiceMonitor` resources to enable Prometheus (running on the management cluster) to probe workload cluster API server endpoints via the Prometheus blackbox exporter.

This provides:
- **Visibility into workload cluster health** from the management cluster perspective
- **Availability metrics** for the Kubernetes API server
- **Response time measurements** for API endpoints
- **Integration with existing Prometheus monitoring** via the prometheus-operator

## Why cluster-probes?

When managing multiple Kubernetes clusters, the management cluster needs visibility into workload cluster API server availability. Rather than requiring agents or complex instrumentation in each workload cluster, cluster-probes uses Prometheus's blackbox exporter to probe the API server endpoints from the management cluster.

This approach:
- Requires no additional deployment in workload clusters
- Integrates naturally with the management cluster's Prometheus
- Follows Kubernetes and GiantSwarm best practices
- Easily scales to monitor many clusters

## Who can use it?

This chart is designed for:
- **GiantSwarm customers** using the GiantSwarm cluster chart - installed automatically via app integration
- **GiantSwarm operators** managing multi-cluster environments
- **Organizations** with Prometheus and blackbox-exporter deployed on management clusters

## Installing

### For GiantSwarm Customers

This app is typically installed automatically when deploying a workload cluster via the cluster chart. Configuration is minimal - values are passed from the cluster chart at runtime.

### Manual Installation

For detailed installation instructions and configuration options, see [helm/cluster-probes/README.md](helm/cluster-probes/README.md).

```bash
# Install from OCI registry
helm install cluster-probes oci://gsoci.azurecr.io/charts/giantswarm/cluster-probes \
  --version 0.1.0 \
  --namespace cluster \
  --set clusterName=my-cluster \
  --set baseDomain=awsprod.gigantic.io
```

## Configuration

Key configuration options:
- **clusterName**: The workload cluster being monitored
- **baseDomain**: Base domain for API endpoint construction
- **apiServerPort**: API server port (default: 443)
- **interval**: Probe interval (default: 60s)
- **module**: Blackbox module to use (default: http_2xx_insecure)

See [helm/cluster-probes/README.md](helm/cluster-probes/README.md) for comprehensive configuration guide.

## Integration

This chart is integrated into [giantswarm/cluster](https://github.com/giantswarm/cluster) as a managed HelmRelease. When a workload cluster is created, cluster-probes is automatically deployed to monitor the API server.

## Repository Structure

```
cluster-probes/
├── .circleci/              # CircleCI CI/CD pipeline
├── .github/                # GitHub workflows and templates
├── .abs/                   # App build system configuration
├── helm/
│   └── cluster-probes/     # Main Helm chart
│       ├── Chart.yaml
│       ├── values.yaml
│       └── templates/
│           ├── _helpers.tpl
│           └── servicemonitor.yaml
├── CHANGELOG.md            # Version history
├── CODE_OF_CONDUCT.md      # Community guidelines
├── CODEOWNERS              # Team ownership
├── LICENSE                 # Apache 2.0 license
└── README.md               # This file
```

## Development

### Prerequisites
- Kubernetes >= 1.24
- Helm >= 3.0
- Prometheus Operator (for ServiceMonitor CRD)
- Prometheus Blackbox Exporter

### Testing

```bash
# Lint the chart
helm lint helm/cluster-probes/

# Validate templating
helm template cluster-probes helm/cluster-probes/ | kubectl apply --dry-run=client -f -

# With custom values
helm template cluster-probes helm/cluster-probes/ -f custom-values.yaml
```

### Building and Publishing

The chart is automatically built and published to the OCI registry on GitHub releases via CircleCI.

```bash
# Locally build the chart
helm package helm/cluster-probes/

# The package is published to:
# oci://gsoci.azurecr.io/charts/giantswarm/cluster-probes
```

## Versioning

This project uses semantic versioning:
- **MAJOR**: Breaking changes
- **MINOR**: New features (backward compatible)
- **PATCH**: Bug fixes

Current version: `0.1.0`

## Contributing

We welcome contributions! Please see [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) for our community standards and guidelines for contributing.

## Support

For issues, questions, or suggestions:
- Open an issue on [GitHub](https://github.com/giantswarm/cluster-probes/issues)
- Contact the GiantSwarm Phoenix team
- See [SECURITY.md](SECURITY.md) for security-related issues

## License

[Apache License 2.0](LICENSE)


- [Using GitOps to instantiate the App](https://docs.giantswarm.io/tutorials/continuous-deployment/apps/add-appcr/)
- By creating an [App resource](https://docs.giantswarm.io/reference/platform-api/crd/apps.application.giantswarm.io) using the platform API as explained in [Getting started with App Platform](https://docs.giantswarm.io/tutorials/fleet-management/app-platform/).

## Configuring

### values.yaml

**This is an example of a values file you could upload using our web interface.**

```yaml
# values.yaml

```

### Sample App CR and ConfigMap for the management cluster

If you have access to the Kubernetes API on the management cluster, you could create the App CR and ConfigMap directly.

Here is an example that would install the app to workload cluster `abc12`:

```yaml
# appCR.yaml

```

```yaml
# user-values-configmap.yaml

```

See our [full reference on how to configure apps](https://docs.giantswarm.io/tutorials/fleet-management/app-platform/app-configuration/) for more details.

## Compatibility

This app has been tested to work with the following workload cluster release versions:

- _add release version_

## Limitations

Some apps have restrictions on how they can be deployed.
Not following these limitations will most likely result in a broken deployment.

- _add limitation_

## Credit

- {APP HELM REPOSITORY}

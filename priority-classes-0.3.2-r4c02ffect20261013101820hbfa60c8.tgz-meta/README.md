# priority-classes

Helm chart that provides standardized PriorityClass resources for Giant Swarm clusters.

## Overview

This chart deploys two PriorityClass resources as defined by sig-architecture:

- **`giantswarm-critical`** (value: 1000000000) - For components that the cluster absolutely requires to run and Giant Swarm is adding (e.g., kyverno)
- **`giantswarm-high`** (value: 900000000) - For important components that should preempt customer workloads (e.g., observability components, crossplane)

These priority classes replace the previous inconsistent per-app priority classes (like `crossplane-critical`, `flux-giantswarm-flux-giantswarm`, `prometheus`) to ensure consistency across all clusters.

**Note:** `system-cluster-critical` and `system-node-critical` are built-in Kubernetes priority classes and are not included in this chart.

## Priority Order

1. `system-node-critical` (2000001000) - Kubernetes built-in
2. `system-cluster-critical` (2000000000) - Kubernetes built-in  
3. `giantswarm-critical` (1000000000) - This chart
4. `giantswarm-high` (900000000) - This chart

## Background

See [roadmap issue #3483](https://github.com/giantswarm/roadmap/issues/3483) for the full context and sig-architecture decision.

## Installing

This chart is automatically deployed to workload clusters via HelmRelease when enabled in the cluster configuration.

## Configuration

### values.yaml

The priority class values are fixed as per sig-architecture decision and should not be changed:

```yaml
priorityClasses:
  giantswarmCritical:
    enabled: true
    value: 1000000000
    description: "Stuff that the cluster absolutely requires to run and Giant Swarm is adding (e.g., kyverno)"
  giantswarmHigh:
    enabled: true
    value: 900000000
    description: "Stuff that should preempt customer workloads (e.g., observability components, crossplane)"
```

## Migration

Apps should migrate from legacy priority classes:

| Legacy Priority Class | Value | Replace With |
|----------------------|-------|--------------|
| `crossplane-critical` | 600000000 | `giantswarm-high` |
| `flux-giantswarm-flux-giantswarm` | 1000000000 | `giantswarm-critical` or `giantswarm-high` |
| `prometheus` | 500000000 | `giantswarm-high` |

## Compatibility

This chart is designed to work with all Giant Swarm CAPI-based clusters.

## Credit

- Defined by Giant Swarm sig-architecture
- Reference: https://github.com/giantswarm/roadmap/issues/3483

[![CircleCI](https://circleci.com/gh/giantswarm/teleport-kube-agent-app.svg?style=shield)](https://circleci.com/gh/giantswarm/teleport-kube-agent-app)

# teleport-kube-agent

This repository packages the upstream [`teleport-kube-agent`](https://github.com/gravitational/teleport/tree/master/examples/chart/teleport-kube-agent) Helm chart as a Giant Swarm app.
The agent runs in every Giant Swarm cluster and registers the cluster's Kubernetes API with Giant Swarm's Teleport cluster, which is how Giant Swarm staff reach management and workload clusters.
The wrapper adds Giant Swarm registry, Pod Security Standards and HTTP proxy settings on top of the upstream chart.

## Installing

The app is installed into `kube-system` of every cluster by the [`cluster` chart](https://github.com/giantswarm/cluster) as a Flux `HelmRelease`.
The join token, proxy address and cluster name come from [`teleport-operator`](https://github.com/giantswarm/teleport-operator), which writes them into a per-cluster ConfigMap that the HelmRelease reads.
It is not intended to be installed by customers; to run it elsewhere, deploy it like any other Giant Swarm app with a [Flux HelmRelease](https://docs.giantswarm.io/tutorials/fleet-management/app-platform/deploy-app-helmrelease/) or [`kubectl gs deploy chart`](https://docs.giantswarm.io/reference/kubectl-gs/deploy-chart/).

## Configuring

All upstream values are nested under `teleport-kube-agent`; see the [upstream chart reference](https://goteleport.com/docs/enroll-resources/agents/kubernetes/) for the full list.
The top-level `proxy` block is only needed for clusters that reach the internet through an HTTP proxy.

```yaml
# values.yaml
teleport-kube-agent:
  roles: kube
  proxyAddr: teleport.example.com:443
  kubeClusterName: abc12
  joinParams:
    method: token
    tokenName: <join token>

proxy:
  http: http://proxy.example.com:3128
  https: http://proxy.example.com:3128
  noProxy: 10.0.0.0/8,.svc,.cluster.local
```

See our [full reference on how to configure apps](https://docs.giantswarm.io/tutorials/fleet-management/app-platform/app-configuration/) for more details.

## Development

The upstream chart is vendored into `helm/teleport-kube-agent/charts/teleport-kube-agent` with [vendir](https://carvel.dev/vendir/) (see [`vendir.yml`](vendir.yml)), and Giant Swarm changes are kept as patches in [`patches/`](patches/).
Run `make update-chart` to sync a new upstream version and re-apply the patches, and `make regenerate-patches` after changing the vendored chart.

## Credit

- [gravitational/teleport](https://github.com/gravitational/teleport), whose `teleport-kube-agent` chart this app wraps.

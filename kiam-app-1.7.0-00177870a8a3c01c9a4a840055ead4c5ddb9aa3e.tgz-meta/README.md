[![CircleCI](https://circleci.com/gh/giantswarm/kiam-app.svg?style=shield)](https://circleci.com/gh/giantswarm/kiam-app)

# kiam-app

Helm chart for [kiam] service running in tenant clusters.

## Installing the Chart

To install the chart locally:

```bash
$ git clone https://github.com/giantswarm/kiam-app.git
$ cd kiam-app
$ helm install helm/kiam-app
```

Provide a custom `values.yaml`:

```bash
$ helm install kiam-app -f values.yaml
```

Deployment to Tenant Clusters is handled by [app-operator](https://github.com/giantswarm/app-operator).

## Configuration

Configuration options are documented in [Configuration.md](helm/kiam-app/Configuration.md) document.

## Release Process

* Ensure CHANGELOG.md is up to date.
* Create a new GitHub release with the version e.g. `v0.1.0` and link the
changelog entry.
* This will push a new git tag and trigger a new tarball to be pushed to the
[default-catalog].

[app-operator]: https://github.com/giantswarm/app-operator
[default-catalog]: https://github.com/giantswarm/default-catalog
[default-test-catalog]: https://github.com/giantswarm/default-test-catalog
[kiam]: https://github.com/uswitch/kiam

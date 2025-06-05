[![CircleCI](https://circleci.com/gh/giantswarm/cilium-app.svg?style=shield)](https://circleci.com/gh/giantswarm/cilium-app)

# cilium chart

Giant Swarm offers a cilium App which can be installed in workload clusters.

## Installing

There are several ways to install this app onto a workload cluster.

- [Using our web interface](https://docs.giantswarm.io/ui-api/web/app-platform/#installing-an-app).
- By creating an [App resource](https://docs.giantswarm.io/ui-api/management-api/crd/apps.application.giantswarm.io/) in the management cluster as explained in [Getting started with App Platform](https://docs.giantswarm.io/app-platform/getting-started/).

## Upgrading cilium version

Update the version in `vendir.s1.yml` and run `./sync/sync.sh`. It's good to check the diffs from upstream. They are stored in the `./diffs` directory.


## Version compatibility

| Cilium app | Cilium version | 
| --- | --- |
| 0.25.x | 1.15.x |
| 0.26.x | 1.16.x |
| 0.27.x | 1.16.x |
| 0.28.x | 1.16.x |
| 0.29.x | 1.16.x |
| 0.30.x | 1.16.x |
| 0.31.x | 1.16.x |
| 0.32.x | 1.17.x |
| 1.0.x | 1.17.x |
| 1.1.x | 1.17.x |
| 1.2.x | 1.17.x |

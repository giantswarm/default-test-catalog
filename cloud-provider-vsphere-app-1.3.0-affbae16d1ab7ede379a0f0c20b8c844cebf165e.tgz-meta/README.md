[![CircleCI](https://circleci.com/gh/giantswarm/cloud-provider-vsphere-app-app.svg?style=shield)](https://circleci.com/gh/giantswarm/cloud-provider-vsphere-app-app)

# cloud-provider-vsphere chart

Giant Swarm offers a cloud-provider-vsphere App which can be installed in workload clusters.
Here we define the cloud-provider-vsphere chart with its templates and default configuration.

**What is this app?**

`cloud-provider-vsphere` allows clusters to interface with vSphere.

## Compatibility matrix

This app tracks the [upstream cloud provider repo](https://github.com/kubernetes/cloud-provider-vsphere). The following matrix
details which version of this app should be used with different Kubernetes versions.

| app version | kubernetes version | upstream tag |
| ----------- | ------------------ | ------------ |
| v1.x.x      | 1.22.x             | v1.22.x      |

## Limitations

Some apps have restrictions on how they can be deployed.
Not following these limitations will most likely result in a broken deployment.

* Restricted to vSphere clusters only.

## Credit

* https://github.com/kubernetes/cloud-provider-vsphere

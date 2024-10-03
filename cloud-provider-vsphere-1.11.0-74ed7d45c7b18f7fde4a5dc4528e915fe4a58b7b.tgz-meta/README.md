[![CircleCI](https://circleci.com/gh/giantswarm/cloud-provider-vsphere-app.svg?style=shield)](https://circleci.com/gh/giantswarm/cloud-provider-vsphere-app)

# cloud-provider-vsphere-app

This app contains CPI and CSI for CAPV clusters. 

| Cloud Provider vSphere app | Kubernetes version |
| -------------------------- | ------------------ |
| 1.10.x | 1.30.x |
| 1.9.x | 1.29.x |
| 1.8.x | 1.28.x |
| 1.7.x | 1.27.x |

## How to install

- Clone this repository. 
  ```sh
  git clone https://github.com/giantswarm/cloud-provider-vsphere-app.git
  ```
- Go into helm chart directory.
  ```sh
  cd cloud-provider-vsphere-app/helm/cloud-provider-vsphere
  ```
- Fill the missing values in values.yaml file.
  ```sh
  $EDITOR values.yaml
  ```
- Install the chart.
  ```sh
  helm install cloud-provider-vsphere -n kube-system -f values.yaml .
  ```

## Source of charts

The sources of charts are as below.

- CPI: https://github.com/kubernetes/cloud-provider-vsphere/tree/master/charts/vsphere-cpi
- CSI: https://github.com/kubernetes-sigs/vsphere-csi-driver/blob/master/manifests/vanilla/vsphere-csi-driver.yaml
- kube-vip: https://github.com/kube-vip/helm-charts/commit/8c5b2d353082372ac8698885c1dab01efeebec98
- kubevip-cloud-provider: https://github.com/kube-vip/helm-charts/tree/kube-vip-cloud-provider-0.2.2/charts/kube-vip-cloud-provider

CPI's helm chart is not as mature as we want so we customized it.
CSI doesn't have a helm chart so we created the chart based on the manifest file.

It is planned to automate chart creation with `kustomize` to be able to update easily and to be able to track our custom changes properly.

## Information on our charts

### vSphere Cloud Provider

Nothing exotic about this, we just grab the upstream chart.

> [!NOTE]
> The vSphere Cloud Provider versions are aligned with Kubernetes versions for support (e.g. CPI v1.27.0 for Kubernetes v1.27) so make sure the CSI is also aligned when merging.

### vSphere CSI driver

> [!NOTE]
> There is no upstream Helm chart for the CSI, we generate it from the manifests and we apply customizations. There are a few overwrites and patches in the `/config/vsphere-csi-driver` folder.
> Find the latest CSI version that is compatible with the Kubernetes version of the cluster it will run in. You will find this in the [Release Notes](https://docs.vmware.com/en/VMware-vSphere-Container-Storage-Plug-in/3.0/rn/vmware-vsphere-container-storage-plugin-30-release-notes/index.html). For instance, vSphere CSI `v3.2.0` is compatible with Kubernetes `1.27` to `1.29`. (You can check the tag exists in the [upstream repo](https://github.com/kubernetes-sigs/vsphere-csi-driver/tags)). Make sure the CPI is also aligned when merging.

### kube-vip

> [!NOTE]
> New versions of the `kube-vip` Helm chart are released with `kube-vip-x.y.z` in the `kube-vip/helm-charts` repo (same repo as `kube-vip-cloud-provider`).

Note that the upstream `kube-vip` Helm chart isn't well maintained so we use the latest chart but also the latest kube-vip tags.
In `Chart.yaml`, `version` corresponds to the chart's release and `appVersion` is used as kube-vip image tag.

### kube-vip-cloud-provider

> [!NOTE]
> New versions of the `kube-vip-cloud-provider` Helm chart are released with `kube-vip-cloud-provider-x.y.z` in the `kube-vip/helm-charts` repo (same repo as `kube-vip`).

Note that the upstream `kube-vip-cloud-provider` Helm chart isn't well maintained so we use the latest chart but also the latest kube-vip-cloud-provider tags.
In `Chart.yaml`, `version` corresponds to the chart's release and `appVersion` is used as kube-vip-cloud-provider image tag.

## How to update the charts with Renovate

Renovate updates `version` and `appVersion` fields in `Chart.yaml` files located in `./config/xxx/overrides`. This is the file that is then used to template the actual sub-charts.

> [!CAUTION]
> Only release this app if you are sure that both `vSphere Cloud Provider` and `vSphere CSI driver` support the same Kubernetes version.

### Update the charts

- Run `make all`.
- Check the diffs.
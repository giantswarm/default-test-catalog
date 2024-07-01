[![CircleCI](https://circleci.com/gh/giantswarm/cloud-provider-vsphere-app.svg?style=shield)](https://circleci.com/gh/giantswarm/cloud-provider-vsphere-app)

# cloud-provider-vsphere-app

This app contains CPI and CSI for CAPV clusters. 

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

## How to update the charts automatically

### kube-vip

- In the `kube-vip/helm-charts` upstream repo, browse to [charts/kube-vip/Chart.yaml](https://github.com/kube-vip/helm-charts/blob/main/charts/kube-vip/Chart.yaml) (`kube-vip` folder, **not** `kube-vip-cloud-provider`) and switch branch to the latest `kube-vip-x.y.z` tag (e.g. `kube-vip-0.6.1`).
- Edit [update-kubevip-chart.sh](hack/update-kubevip-chart.sh) here and change the branch value under `./hack/clone-git-repo.sh` to reflect the branch version. For instance:

``` sh
"./hack/clone-git-repo.sh" \
    "kube-vip/helm-charts" \
    "kube-vip-0.6.1" \
    "kube-vip"
```

- Change `version` and `appVersion` fields in our [Chart.yaml overwrite](config/kube-vip/overwrites/Chart.yaml) to match the ones in upstream `Chart.yaml` (check you're on the right branch).
- In `Chart.yaml`, `appVersion` is used as kube-vip image tag. If you require a different tag than what is set upstream, change it to the tag you need from the [upstream kube-vip tags](https://github.com/kube-vip/kube-vip/tags) in the [Chart.yaml overwrite](config/kube-vip/overwrites/Chart.yaml).

### kube-vip-cloud-provider

- In the `kube-vip/helm-charts` upstream repo, browse to [charts/kube-vip-cloud-provider/Chart.yaml](https://github.com/kube-vip/helm-charts/blob/main/charts/kube-vip-cloud-provider/Chart.yaml) (`kube-vip-cloud-provider` folder, **not** `kube-vip`) and switch branch to the latest `kube-vip-cloud-provider-x.y.z` tag (e.g. `kube-vip-cloud-provider-0.2.2`).
- Edit [update-kubevip-cloud-provider-chart.sh](hack/update-kubevip-cloud-provider-chart.sh) here and change the branch value under `./hack/clone-git-repo.sh` to reflect the branch version. For instance:

``` sh
"./hack/clone-git-repo.sh" \
    "kube-vip/helm-charts" \
    "kube-vip-cloud-provider-0.2.2" \
    "kube-vip-cloud-provider"
```

- Change `version` and `appVersion` fields in our [Chart.yaml overwrite](config/kube-vip/overwrites/Chart.yaml) to match the ones in upstream `Chart.yaml` (check you're on the right branch).
- In `Chart.yaml`, `appVersion` is used as kube-vip image tag. If you require a different tag than what is set upstream, change it to the tag you need from the [upstream kube-vip-cloud-provider tag](https://github.com/kube-vip/kube-vip-cloud-provider/tags) in the [Chart.yaml overwrite](config/kube-vip-cloud-provider/overwrites/Chart.yaml).

### vSphere CSI driver

> [!NOTE]
> There is no upstream Helm chart for the CSI, we generate it from the manifests and we apply customizations. There are a few overwrites and patches in the `/config/vsphere-csi-driver` folder.

- First off, find the latest CSI version that is compatible with the Kubernetes version of the cluster it will run in. You will find this in the [Release Notes](https://docs.vmware.com/en/VMware-vSphere-Container-Storage-Plug-in/3.0/rn/vmware-vsphere-container-storage-plugin-30-release-notes/index.html). For instance, vSphere CSI `v3.2.0` is compatible with Kubernetes `1.27` to `1.29`. (You can check the tag exists in the [upstream repo](https://github.com/kubernetes-sigs/vsphere-csi-driver/tags))
- Edit [update-csi-chart.sh](hack/update-csi-chart.sh) here and change the branch value under `./hack/clone-git-repo.sh` to reflect the branch version. For instance:

``` sh
"./hack/clone-git-repo.sh" \
  "kubernetes-sigs/vsphere-csi-driver" \
  "v3.2.0" \
  "vsphere-csi-driver"
```

- Then Adjust `version` and `appVersion` in the [Chart.yaml overwrite](config/vsphere-csi-driver/overwrites/Chart.yaml). For instance:

``` yaml
appVersion: 3.2.0
version: 3.2.0
```

### vSphere Cloud Provider

> [!NOTE]
> The vSphere Cloud Provider versions are aligned with Kubernetes versions for support (e.g. CPI v1.27.0 for Kubernetes v1.27).

- In the [upstream repo](https://github.com/kubernetes/cloud-provider-vsphere/tree/release-1.27), look for the branch of the version to update to (`release-x.yy`).
- Edit [update-cpi-chart.sh](hack/update-cpi-chart.sh) here and change the branch value under `./hack/clone-git-repo.sh` to reflect the branch version. For instance:

``` sh
"./hack/clone-git-repo.sh" \
    "/kubernetes/cloud-provider-vsphere" \
    "release-1.27" \
    "cloud-provider-vsphere"
```

- Then Adjust `version` and `appVersion` in the [Chart.yaml overwrite](config/cloud-provider-for-vsphere/overwrites/Chart.yaml). For instance:

``` yaml
appVersion: 1.27.0
version: 1.27.0
```

### Update parent chart's dependencies

- Update each chart's version to use in the parent `Chart.yaml` in [templates/Chart.yaml](templates/Chart.yaml).

### Update the charts

- Run `make all`.
- Check the diffs.
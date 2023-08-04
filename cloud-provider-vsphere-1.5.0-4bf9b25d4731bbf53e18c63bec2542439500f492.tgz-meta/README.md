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

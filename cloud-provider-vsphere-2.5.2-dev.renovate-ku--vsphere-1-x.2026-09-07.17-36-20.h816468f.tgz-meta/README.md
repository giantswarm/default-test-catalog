[![CircleCI](https://circleci.com/gh/giantswarm/cloud-provider-vsphere-app.svg?style=shield)](https://circleci.com/gh/giantswarm/cloud-provider-vsphere-app)

# cloud-provider-vsphere-app

### vSphere Cloud Provider

Source: https://github.com/kubernetes/cloud-provider-vsphere/tree/master/charts/vsphere-cpi

Nothing really exotic about this, we just grab the upstream chart.

> [!NOTE]
> The vSphere Cloud Provider versions are aligned with Kubernetes versions for support (e.g. CPI v1.27.0 for Kubernetes v1.27) so make sure the CSI is also aligned when merging.

## Information on Renovate in this repo

Renovate updates `version` and `appVersion` fields in `Chart.yaml` files located in `./config/xxx/overrides`. This is the file that is then used to template the actual sub-charts.

> [!CAUTION]
> Only release this app if you are sure that both `vSphere Cloud Provider` and `vSphere CSI driver` support the same Kubernetes version.

### Process of updating the charts

- Renovate opens a PR (let's say on branch `renovate/kube-vip-helm-charts-0.x`).
- Pull the branch on your local: `git pull && git checkout renovate/kube-vip-helm-charts-0.x`
- Run the scripts to update the charts: `make all`
- If everything is ok, push the changes back to the branch and test the PR.
- Check the diffs.

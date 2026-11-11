[![CircleCI](https://dl.circleci.com/status-badge/img/gh/giantswarm/azure-private-dns-crossplane-resources/tree/main.svg?style=svg)](https://dl.circleci.com/status-badge/redirect/gh/giantswarm/azure-private-dns-crossplane-resources/tree/main)

# Azure Private DNS Zone Crossplane Resources

This app contains the Crossplane resources needed to create Azure Private DNS Zones and Virtual Network Links on management/workload clusters.

Resources created:
- `<clusterName>-<privateDnsZoneName>` Private DNS Zone for private endpoint resolution
- `<clusterName>-<virtualNetworkLinkName>` Virtual Network Link to associate the DNS zone with the cluster VNet

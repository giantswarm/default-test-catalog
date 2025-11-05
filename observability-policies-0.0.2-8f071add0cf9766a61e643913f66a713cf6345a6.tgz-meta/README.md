# Observability Policies App

This application deploys Kyverno policies specifically designed to protect critical CustomResourceDefinitions (CRDs) within your observability platform from accidental deletion.

## Motivation

The primary motivation for this dedicated application is to avoid introducing a dependency from the `observability-bundle` to the `security-bundle` (where `kyverno-crds` is typically embedded). This promotes modularity and independent management of security and observability components.

## Functionality

When enabled, this Helm chart installs the following:

1.  A `ClusterPolicy` (Kyverno resource, named `<release-name>-prevent-crd-deletion` by default) that actively prevents the `DELETE` operation on a list of specified CRD names.
2.  A `ClusterRole` (named `<release-name>-crd-monitor` by default) that grants the Kyverno background controller the necessary permissions (`get`, `list`, `watch`) to monitor these CRDs. This role is designed to be aggregated to Kyverno's main admission controller role.

## Configuration
The behavior of the chart can be configured through the `helm/observability-policies/values.yaml` file or by using `--set` arguments during Helm installation/upgrade.

| Parameter | Description | Default |
|-----------|-------------|---------|
| `enabled` | Deploy and activate the policies | `true` |
| `resources` | List of CRD names that should be protected from deletion | See below |

Default protected resources (primarily Prometheus Operator CRDs, but configurable for others):
```yaml
resources:
  - alertmanagerconfigs.monitoring.coreos.com
  - alertmanagers.monitoring.coreos.com
  - podmonitors.monitoring.coreos.com
  - probes.monitoring.coreos.com
  - prometheusagents.monitoring.coreos.com
  - prometheuses.monitoring.coreos.com
  - prometheusrules.monitoring.coreos.com
  - scrapeconfigs.monitoring.coreos.com
  - servicemonitors.monitoring.coreos.com
  - thanosrulers.monitoring.coreos.com
```

## Prerequisites

- **Kyverno**: This chart deploys Kyverno policies. Kyverno must be installed and running in your Kubernetes cluster for these policies to be effective.

## Deployment

Deploy the chart using standard Helm commands:

```bash
helm upgrade --install <release-name> helm/observability-policies --namespace <your-namespace>
```

## Verification

Once deployed and enabled, you can verify the policies:

1.  **Ensure Kyverno is running in your cluster.**
2.  **Check the Kyverno policy status:**
    You can list the installed cluster policies. The exact label selector might vary based on your Helm release name and common label setup, but `app.kubernetes.io/instance=<release-name>` is a common pattern.
    ```bash
    # Replace <release-name> with the name of your Helm release
    kubectl get cpol -l app.kubernetes.io/instance=<release-name>
    # The policy name will be similar to <release-name>-observability-policies-prevent-crd-deletion
    kubectl get clusterpolicy -l app.kubernetes.io/instance=<release-name>
    ```

3.  **Attempt to delete one of the protected CRDs.**
    Warning: the operation _should_ be blocked by Kyverno. But if the policy does not work, it will allow the removal of the CRD, and you will lose all associated resources.
    For example, if `prometheuses.monitoring.coreos.com` is listed in your `values.yaml` under `resources`:
    ```bash
    kubectl delete crd prometheuses.monitoring.coreos.com
    ```
    You should receive an error message indicating the request was denied by a Kyverno policy (e.g., `{{ include "observability-policies.fullname" . }}-prevent-crd-deletion`).

## Enabling and Disabling Policies

You can control the deployment of these policies using the `enabled` value:

*   **To disable policies** (if currently enabled):
    ```bash
    helm upgrade --install <release-name> helm/observability-policies --namespace <your-namespace> --set enabled=false
    ```
    Alternatively, you can uninstall the chart:
    ```bash
    helm uninstall <release-name> -n <your-namespace>
    ```

*   **To enable policies** (if currently disabled):
    ```bash
    helm upgrade --install <release-name> helm/observability-policies --namespace <your-namespace> --set enabled=true
    ```


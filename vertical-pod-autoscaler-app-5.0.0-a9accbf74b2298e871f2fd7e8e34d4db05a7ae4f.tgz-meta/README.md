[![CircleCI](https://circleci.com/gh/giantswarm/vertical-pod-autoscaler-app.svg?style=shield)](https://circleci.com/gh/giantswarm/vertical-pod-autoscaler-app)

# Vertical Pod Autoscaler

Giant Swarm offers a vertical-pod-autoscaler Managed App which can be installed in workload clusters. Here we define the vertical-pod-autoscaler chart with its templates and default configuration.

You should read our [public documentation](https://docs.giantswarm.io/getting-started/operations/autoscaling/vertical-pod-autoscaler).

## Example

You can create a `verticalpodautoscaler` or `vpa` resource that will monitor/update other resources in the same namespace.

Syntax looks like this:

```
apiVersion: autoscaling.k8s.io/v1
kind: VerticalPodAutoscaler
metadata:
  name: fluent-logshipping-app
  namespace: monitoring
spec:
  targetRef:
    apiVersion: apps/v1
    kind: DaemonSet
    name: fluent-logshipping-app
  resourcePolicy:
    containerPolicies:
    - containerName: fluent-bit
      controlledResources:
      - memory
      mode: Auto
      minAllowed:
        memory: 100Mi
      maxAllowed:
        memory: 500Mi
  updatePolicy:
    updateMode: Auto
```

This one will look for `fluent-bit` containers from the `fluent-logshipping-app` DaemonSet, and will act on memory. It will automatically adjust the pod's memory `requests` between `100Mi` and `500Mi`.

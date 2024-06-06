# Chart Operator Extensions

Application used to deploy `chart-operator`extensions.

Main reason for this is that `chart-operator` is one of the first thing we install - to for example workload clusters -
and then it installs most of the other apps. So for example we cannot install service monitors with the operator itself
because Prometheus and related CRDs are not present at the time.

## Structure

Values for the chart is structured in the following way:

- Top level: extension category, for example: `monitoring`
- Component level: component type in the extension, for example: `serviceMonitors`
- Target level: identifier of the target the component belongs to, for example: `operator`, meaning it belongs directly
  to `chart-operator` as a service, deployment or pod. It's going to be most likely `operator` all the times.
- The `enabled` flag: each target must have this flag to be able to control deploying the resource in given clusters.

Example:

```yaml
monitoring:
  serviceMonitors:
    operator:
      enabled: true
      # ...
```

## Extensions

### Monitoring

Deploy service monitors for Prometheus to scrape metrics from `chart-operator`.

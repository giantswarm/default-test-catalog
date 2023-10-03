# Chart Operator ServiceMonitors

Application used to deploy `chart-operator` service monitors.

Main reason for this is that `chart-operator` is one of the first thing we install - to for example workload clusters -
and then it installs most of the other apps. So for example we cannot install service monitors with the operator itself
because Prometheus and related CRDs are not present at the time.

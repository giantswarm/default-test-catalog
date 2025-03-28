# observability-policies App

Application used to deploy Kyverno policies dedicated to the observability stack.

Main reason for this is to avoid introducing a dependency in the `observability-bundle` towards the `security-bundle` where `kyverno-crds` is embedded.

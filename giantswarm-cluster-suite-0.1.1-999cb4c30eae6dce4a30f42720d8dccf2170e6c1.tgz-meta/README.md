[![CircleCI](https://dl.circleci.com/status-badge/img/gh/giantswarm/{APP-NAME}/tree/main.svg?style=svg)](https://dl.circleci.com/status-badge/redirect/gh/giantswarm/giantswarm-cluster-suite/tree/main)

[Read me after cloning this template (GS staff only)](https://handbook.giantswarm.io/docs/dev-and-releng/app-developer-processes/adding_app_to_appcatalog/)

# giantswarm-cluster-suite chart

This app is meant part of cluster default apps, which delivers the basic set of resources needed by other apps. These resources currently encompass:

* `giantswarm` Namespace
* `giantswarm-critical` PriorityClass


# Helm

Helm is a tool for managing Kubernetes charts. Charts are packages of pre-configured Kubernetes resources.

Use Helm to:

- Find and use popular software packaged as Helm charts to run in Kubernetes
- Share your own applications as Helm charts
- Create reproducible builds of your Kubernetes applications
- Intelligently manage your Kubernetes manifest files
- Manage releases of Helm packages


## Install 

There are 2 part : Helm Client (heml) and Helm Server (Tiller)

### Helm client

- Download your [desired version](https://github.com/helm/helm/releases)
- Unpack (`tar -zxvf helm-vx.x.x-linux-amd64.tgz`)
- Move `helm` binary to its desired destination (`mv linux-amd64/helm /usr/local/bin/helm`)

### Tiller

The easiest way: 

`helm init`

## Charts 

Helm uses a packaging format called charts. 

### Structure directory 

Example with `wordpress`

```
wordpress/
  Chart.yaml          # A YAML file containing information about the chart
  LICENSE             # OPTIONAL: A plain text file containing the license for the chart
  README.md           # OPTIONAL: A human-readable README file
  requirements.yaml   # OPTIONAL: A YAML file listing dependencies for the chart
  values.yaml         # The default configuration values for this chart
  charts/             # A directory containing any charts upon which this chart depends.
  templates/          # A directory of templates that, when combined with values,
                      # will generate valid Kubernetes manifest files.
  templates/NOTES.txt # OPTIONAL: A plain text file containing short usage notes
```

## Helm commands

Common actions from this point include:

- helm search: search for charts
- helm fetch: download a chart to your local directory to view
- helm install: upload the chart to Kubernetes
- helm list: list releases of charts

### See more 

https://docs.helm.sh/helm/#see-also


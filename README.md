# demo-helm-chart

This repository contains a helm chart example using a custom `demo-library` chart.

More about the library chart in my [other repo]()

# Update helm dependencies

[official docs](https://helm.sh/docs/helm/helm_dependency_update/)

This command will update and download your custom helm library charts and other dependencies.

In this case, it will download my `demo-library` chart from my OCI public repo.

Look at this dependency: [Chart.yaml#30](./Chart.yaml#30)

```bash
helm dependency update .
```

Feel free to render this chart locally to see what happened and how ConfigMap rendered from the library chart:

```bash
helm template .
```

example:

```bash
$ helm template test-release .
---
# Source: demo-chart/templates/configmap.yaml
apiVersion: v1
data:
  myvalue: Hello World
kind: ConfigMap
metadata:
  labels: demo-chart-test-release
  name: demo-chart-test-release
```

## Package helm chart

If you are creating your charts using a library charts, you need to push your charts to an OCI registry as well.

This part is similar to a [library chart instructions](https://github.com/ksemele/demo-helm-library) and uses similar auth in your OCI repo.

So at the first you need [update helm dependencies](#update-helm-dependencies)

Then just package it:

```bash
helm package .
```

example:

```bash
$ helm package .
Successfully packaged chart and saved it to: /path/to/demo-chart-0.1.0.tgz
```

## Push chart to oci repo

```bash
helm push demo-chart-0.1.0.tgz oci://quay.io/$QUAY_USERNAME/test-helm
```

example:

```bash
$ helm push demo-chart-0.1.0.tgz oci://quay.io/$QUAY_USERNAME/test-helm
Pushed: quay.io/greengrunt/test-helm/demo-chart:0.1.0
Digest: sha256:31627af4fbe50626f7759ba00eaf1ffbb509433881d711598c847b5c390b28c6
```

## Additional info

If you are creating a CI/CD pipeline or doing manual builds, don't forget to delete archives, lock file and `charts/` after a successful push in the registry.

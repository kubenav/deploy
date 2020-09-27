# Deploy kubenav at Kubernetes

kubenav is not only available for desktop and mobile, it can also be deployed to your Kubernetes cluster. The following page shows how kubenav can be deployed using [Kustomize](https://kustomize.io) or [Helm](https://helm.sh).

More information for the usage of the web version of kubenav can be found in the [documentation](https://docs.kubenav.io) at [https://docs.kubenav.io/web/getting-started/](https://docs.kubenav.io/web/getting-started/).

## Contributing

Each contribution to kubenav is welcome. If you make changes to the Helm chart you have to bump the `version` in the [`helm/Chart.yaml`](https://github.com/kubenav/deploy/blob/master/helm/Chart.yaml) file. Then a new version of the Helm chart is automatically published when your PR is merged into the master branch.

When you add a new value to the [`helm/values.yaml`](https://github.com/kubenav/deploy/blob/master/helm/values.yaml) file, please also adjust the [documentation](https://docs.kubenav.io/web/helm/).

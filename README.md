# Deploy kubenav at Kubernetes

kubenav is not only available for desktop and mobile, it can also be deployed to your Kubernetes cluster. The following page shows how kubenav can be deployed using [Kustomize](https://kustomize.io) or [Helm](https://helm.sh).

## Kustomize

You can deploy kubenav into your Kubernetes cluster via [Kustomize](https://kustomize.io):

```sh
kubectl apply --kustomize github.com/kubenav/deploy/kustomize
```

To access kubenav run the following:

```sh
kubectl port-forward --namespace kubenav svc/kubenav 14122
```

### Configure Access to Multiple Clusters

The deployment shown in this repository deploys kubenav with the `--incluster` flag. This means kubenav has only access to the cluster were it is deployed in. To deploy kubenav with access to multiple cluster you have to mount a Kubeconfig file into the kubenav container and add the `--kubeconfig` flag.

The following example shows the changes you have to made to the `deployment.yaml` file to access multiple clusters from the same kubenav instance:

```yaml
spec:
  template:
    spec:
      containers:
        - name: kubenav
          args:
             - --kubeconfig=/kubenav/kubeconfig/kubeconfig
          volumeMounts:
            - name: kubeconfig
              mountPath: '/kubenav/kubeconfig'
              readOnly: true
      volumes:
        - name: kubeconfig
          secret:
            secretName: kubeconfig
```

The above example mounts a secret named `kubeconfig` into the kubenav pod and makes use of the Kubeconfig via the `--kubeconfig` flag. The secret must look as follows:

```yaml
---
apiVersion: v1
kind: Secret
metadata:
  name: kubeconfig
data:
  kubeconfig: <REPLACE WITH YOUR BASE64 ENCODED KUBECONFIG>
type: Opaque
```

The value for the `data.kubeconfig` key can be created with `cat ~/.kube/config | base64`.

## Helm

You can deploy kubenav into your Kubernetes cluster via [Helm](https://helm.sh):

```sh
helm repo add kubenav https://kubenav.github.io/helm-repository
helm repo update

kubectl create namespace kubenav
helm upgrade --install --namespace kubenav kubenav kubenav/kubenav
```

### Values

| Value | Description | Default |
| ----- | ----------- | ------- |
| `replicaCount` | Number of replicas which should be created. | `1` |
| `image.repository` | The repository of the Docker image. | `kubenav/kubenav` |
| `image.tag` | The tag of the Docker image which should be used. | `4a5f72c1` |
| `image.pullPolicy` | The pull policy for the Docker image, | `IfNotPresent` |
| `imagePullSecrets` | Secrets which can be used to pull the Docker image. | `[]` |
| `nameOverride` | Expand the name of the chart. | `""` |
| `fullnameOverride` | Override the name of the app. | `""` |
| `deployment.mode` | Set the mode how kubenav should be deployed. This must be `incluster` or `kubeconfig`. | `incluster` |
| `deployment.clusterName` | The name which should be in the frontend, when the `incluster` mode is used. | `kubenav` |
| `deployment.kubeconfig` | When the `kubeconfig` mode is used. This must be a base64 encoded Kubeconfig file. | `""` |
| `rbac.create` | Create the cluster role and cluster role binding. | `true` |
| `rbac.name` | The name of the cluster role and cluster role binding, which should be created/used by kubenav. | `kubenav` |
| `rbac.name` | The permissions which kubenav should have. This must be `admin` or `viewer`. | `admin` |
| `serviceAccount.create` | Create the service account. | `true` |
| `serviceAccount.annotations` | Additional annotations for the service account. | `true` |
| `serviceAccount.name` | The name of the service account, which should be created/used by kubenav. | `kubenav` |
| `podSecurityContext` | Security context for the kubenav pod. | `{}` |
| `securityContext` | Security context for the kubenav container. | `{}` |
| `service.type` | Type of the service which is created. | `ClusterIP` |
| `service.port` | Port of the service which is created. | `80` |
| `ingress.enabled` | Create an ingress. | `false` |
| `ingress.annotations` | Additional annotations for the ingress. | `{}` |
| `ingress.hosts` | Hosts for the ingress. | `[]` |
| `ingress.tls` | TLS configuration for the ingress. | `[]` |
| `resources` | Set resources for the operator. | `{}` |
| `nodeSelector` | Set a node selector. | `{}` |
| `tolerations` | Set tolerations. | `[]` |
| `affinity` | Set affinity. | `{}` |

## Contribution

Each contribution to kubenav is welcome. If you make changes to the Helm chart you have to bump the `version` in the `helm/Chart.yaml` file. Then a new version of the Helm chart is automatically published when your PR is merged into the master branch.

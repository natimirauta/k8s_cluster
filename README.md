# k8s_cluster

Under the `swa` directory, there is a `chart` and a `yaml` directory.

In the `yaml` directory there are all the `.yaml` configs for the application to be deployed in a k8s cluster.

In the `chart` directory there's a k8s chart to deploy the application with `helmfile` tool and `helmfile.yml` located at the root of the repository.

## swa/yaml

First, create all the k8s objects defined under the `swa/yaml` direcotry.

Then deploy the ingress: NGINX ingress can be deployed with Helm: https://kubernetes.github.io/ingress-nginx/deploy/

>helm upgrade --install ingress-nginx ingress-nginx --repo https://kubernetes.github.io/ingress-nginx --namespace ingress-nginx --create-namespace

## swa/chart/simple-webapp

The helm chart will deploy both the application and the ingress in the same namespace.

With the `helmfile` tool, it could be possible for the ingress and the application to be deployed in separate namespaces.

However, the `ingress-nginx` dependency under `swa/chart/Chart.yaml` will need to be removed and specified in `helmfile.yml` instead (with the aforementioned config to have separate namespaces). If not, `ingress-nginx` will be deployed twice: once in the application's namespace and once in the namespaces specified in `helmfile.yml`.

First, install the `helm-diff` plugin with:

>helm plugin install https://github.com/databus23/helm-diff

Install the `helmfile` tool by downloading the binary from: `https://github.com/helmfile/helmfile/releases`.

Move the extracted tool to a path specified in `$PATH` and run:

>helmfile apply --file helmfile.yml

## Test

In order to test and see that the application is deployed correctly, you can port forward the NGINX service:

`kubectl -n <nginx-namespace> --address 0.0.0.0 port-forward svc/<nginx-controller-name> 8080:80`

ex:
>kubectl -n ingress-nginx --address 0.0.0.0 port-forward svc/ingress-nginx-controller 8080:80

...and open up a new browser tab at `localhost:8080`

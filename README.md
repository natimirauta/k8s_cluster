# k8s_cluster

Under the `swa` directory, there are config files for the `simple_web_app` application to be deployed in a Kubernetes cluster.

## SWA

This application uses an NGINX ingress deployed with Helm: https://kubernetes.github.io/ingress-nginx/deploy/

>helm upgrade --install ingress-nginx ingress-nginx --repo https://kubernetes.github.io/ingress-nginx --namespace ingress-nginx --create-namespace

In order to test and see that the application is deployed correctly, you can port forward the NGINX service:

>kubectl -n ingress-nginx --address 0.0.0.0 port-forward svc/ingress-nginx-controller 8080:80

...and open up a new tab at `localhost:8080`

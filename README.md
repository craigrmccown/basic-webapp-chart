# Basic Webapp Chart

This chart creates the necessary resources to deploy a basic, load-balanced web application.

## Installing and Uninstalling

To install this chart, first clone this repository. Then, run:

```console
$ helm install --name release-name ./path-to-repo
```

After the chart is installed, you can uninstall it by running:

```console
$ helm delete --purge release-name
```

## Configuration

To configure this chart, pass the `--set key=value` argument to `helm install` or `helm upgrade`.

| Parameter           | Description                                                           | Default                                                                                                                                 |
| ------------------- | --------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| letsencrypt.server  | The url of the Let's Encrypt server to use                            | `https://acme-staging-v02.api.letsencrypt.org/directory` (Set this to `https://acme-v02.api.letsencrypt.org/directory` for production)  |
| letsencrypt.email   | The email address that will receive Let's Encrypt notification emails | `craigrmccown@gmail.com`                                                                                                                |
| postgresql.enabled  | Whether or not to install a PostgreSQL instance                       | `false`                                                                                                                                 |

## Dependencies

This chart creates few resources of its own, as the main purpose of this chart is to bootstrap these dependencies onto a cluster:

- [cert-manager](https://github.com/helm/charts/tree/master/stable/cert-manager): Automatically issues and renews TLS certificates
- [nginx-ingress](https://github.com/helm/charts/tree/master/stable/nginx-ingress): Bootstraps the Nginx Ingress controller, which is used to proxy ingress traffic to services within the cluster
- [postgresql](https://github.com/helm/charts/tree/master/stable/postgresql): Conditionally installs a PostgreSQL instance

> **Note**: To configure these charts, simply prepend the name of the chart when using the `--set` argument. For example, to set the `defaultBackend.enabled` value for the Nginx Ingress controller, use the key `nginx-ingress.defaultBackend.enabled`.

## Google Cloud

When installing on a GKE cluster, a Load Balancer and external IP address will be automatically provisioned for the Nginx Ingress controller. Make sure to switch the IP address from "ephemeral" to "static". This is the IP address to add to your DNS records if you are pointing a domain name to this cluster.

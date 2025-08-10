# AdGuardHome Sync Chart

A helm chart for deploying AdGuard home sync 

## Prerequisites

Before installing the chart, ensure you have the following tools installed:

- [Kubernetes Cluster](https://kubernetes.io/)
- [Helm](https://helm.sh/)


## Installation 

To install the chart with the release name `my-release`:

```bash
helm repo add adguardsync oci://ghrc.io/PhilIngGood/adguardsync-chart
``` 

This helm chart follows GitOps principle to manage secret. As AdGuardHome password are needed to synchronize them, it is mandatory to store them in a secret, and this secret needs not to be pushed in a remote repository.

To create your secret, you can use the following command

```bash
kubectl create secret generic adh-sync-secret --from-literal=origin='originpassword' --from-literal=replica1='replica1password' --from-literal=api='apipassword'
``` 

Then install your release

```bash
helm install my-release PhilIngGood/adguardsync-chart -f values.yaml
``` 

## Uninstallation

To uninstall your release:
```bash
helm uninstall my-release
``` 

If you have hard time to remember your release name, you can use `helm list` to help you find it
```bash
helm list -A
```

## Configuration 

This chart should work out of the box with default values. If it isn't, please open an issue.
Here are the default values defined in values.yaml:
## Default Values

Here are the default values defined in `values.yaml`:

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `replicaCount` | int | `1` | Number of replicas |
| `image.repository` | string | `"ghcr.io/bakito/adguardhome-sync"` | Container image repository |
| `image.pullPolicy` | string | `"IfNotPresent"` | Image pull policy |
| `image.tag` | string | `"latest"` | Image tag |
| `imagePullSecrets` | list | `[]` | Secrets for pulling images |
| `nameOverride` | string | `""` | Override the name |
| `fullnameOverride` | string | `""` | Override the full name |
| `serviceAccount.create` | bool | `true` | Specify whether a service account should be created |
| `serviceAccount.automount` | bool | `true` | Automatically mount a ServiceAccount's API credentials |
| `serviceAccount.annotations` | object | `{}` | Annotations to add to the service account |
| `serviceAccount.name` | string | `""` | The name of the service account to use |
| `podAnnotations` | object | `{}` | Annotations to add to the pod |
| `podLabels` | object | `{}` | Labels to add to the pod |
| `podSecurityContext.runAsUser` | int | `1000` | User ID to run the pod |
| `podSecurityContext.fsGroup` | int | `1000` | Group ID for the filesystem |
| `podSecurityContext.runAsNonRoot` | bool | `true` | Run as non-root user |
| `podSecurityContext.seccompProfile.type` | string | `"RuntimeDefault"` | Seccomp profile type |
| `securityContext.capabilities.drop` | list | `["ALL"]` | Drop all capabilities |
| `securityContext.readOnlyRootFilesystem` | bool | `false` | Use a read-only root filesystem |
| `securityContext.runAsNonRoot` | bool | `true` | Run as non-root user |
| `securityContext.runAsUser` | int | `1000` | User ID to run the container |
| `securityContext.allowPrivilegeEscalation` | bool | `false` | Allow privilege escalation |
| `resources` | object | `{}` | Resource limits and requests |
| `livenessProbe.httpGet.path` | string | `"/"` | Path for liveness probe |
| `livenessProbe.httpGet.port` | string | `"http"` | Port for liveness probe |
| `readinessProbe.httpGet.path` | string | `"/"` | Path for readiness probe |
| `readinessProbe.httpGet.port` | string | `"http"` | Port for readiness probe |
| `volumes` | list | `[]` | Additional volumes |
| `volumeMounts` | list | `[]` | Additional volume mounts |
| `config.cron` | string | `"0 */2 * * *"` | Cron expression for sync |
| `config.runOnStart` | bool | `true` | Run synchronization on startup |
| `config.continueOnError` | bool | `true` | Continue on error |
| `config.origin.url` | string | `"http://primarydns"` | Origin instance URL |
| `config.origin.apiPath` | string | `"/control"` | API path |
| `config.origin.insecureSkipVerify` | bool | `true` | Skip TLS verification |
| `config.origin.username` | string | `"username"` | Username for origin |
| `config.origin.password.secretKeyRef.name` | string | `"adh-sync-secret"` | Secret name for origin password |
| `config.origin.password.secretKeyRef.key` | string | `"origin"` | Key for origin password |
| `config.replicas` | list | `[{"url": "http://replica1", "username": "username", "password": {"secretKeyRef": {"name": "adh-sync-secret", "key": "replica1"}}, "autosetup": true, "webURL": ""}]` | List of replica configurations |
| `config.api.enabled` | bool | `true` | Enable API |
| `config.api.port` | int | `8080` | Port for API |
| `config.api.username` | string | `"username"` | Username for API |
| `config.api.password.secretKeyRef.name` | string | `"adh-sync-secret"` | Secret name for API password |
| `config.api.password.secretKeyRef.key` | string | `"api"` | Key for API password |
| `config.api.darkMode` | bool | `true` | Enable dark mode for API |
| `config.api.metrics.enabled` | bool | `false` | Enable metrics |
| `config.api.metrics.scrapeInterval` | string | `"60s"` | Scrape interval for metrics |
| `config.api.metrics.queryLogLimit` | int | `10000` | Query log limit |
| `config.features.generalSettings` | bool | `true` | Enable general settings sync |
| `config.features.queryLogConfig` | bool | `true` | Enable query log config sync |
| `config.features.statsConfig` | bool | `true` | Enable stats config sync |
| `config.features.clientSettings` | bool | `true` | Enable client settings sync |
| `config.features.services` | bool | `true` | Enable services sync |
| `config.features.filters` | bool | `true` | Enable filters sync |
| `config.features.dhcp.serverConfig` | bool | `true` | Enable DHCP server config sync |
| `config.features.dhcp.staticLeases` | bool | `true` | Enable DHCP static leases sync |
| `config.features.dns.serverConfig` | bool | `true` | Enable DNS server config sync |
| `config.features.dns.accessLists` | bool | `true` | Enable DNS access lists sync |
| `config.features.dns.rewrites` | bool | `true` | Enable DNS rewrites sync |
| `service.type` | string | `"ClusterIP"` | Kubernetes service type |
| `service.port` | int | `80` | Service port |
| `nodeSelector` | object | `{}` | Node selector |
| `tolerations` | list | `[]` | Tolerations |
| `affinity` | object | `{}` | Affinity rules |
| `ingressRoute.enabled` | bool | `false` | Enable IngressRoute |
| `ingressRoute.annotations` | object | `{}` | Annotations for IngressRoute |
| `ingressRoute.entrypoints` | list | `["websecure"]` | Entry points for IngressRoute |
| `ingressRoute.match` | string | `"Host('path')"` | Match rule for IngressRoute |


## Contributing

Contributions are welcome! But I'm not a developper so I'm still not used to work collaboratively on github. Don't hesitate to open an issue so we can discuss about it.
# FlareSolverr Helm Chart

Helm chart to deploy [FlareSolverr](https://github.com/FlareSolverr/FlareSolverr) on Kubernetes.

FlareSolverr is a proxy server to bypass Cloudflare and DDoS-GUARD protection.

## Installation

```bash
helm repo add mikinhas https://mikinhas.github.io/helm-charts
helm install flaresolverr mikinhas/flaresolverr
```

## Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `replicaCount` | Number of replicas | `1` |
| `image.repository` | Docker image | `flaresolverr/flaresolverr` |
| `image.tag` | Image tag | `v3.4.2` |
| `image.pullPolicy` | Pull policy | `IfNotPresent` |
| `service.type` | Service type | `ClusterIP` |
| `service.port` | Service port | `80` |
| `ingress.enabled` | Enable ingress | `false` |
| `persistence.enabled` | Enable persistence | `false` |
| `resources` | CPU/Memory resource limits | `{}` |
| `podEnvVars` | Environment variables | `[]` |

### Environment variables

```yaml
podEnvVars:
  - name: PUID
    value: "1000"
  - name: PGID
    value: "1000"
  - name: TZ
    value: "Europe/Paris"
```

### Ingress

```yaml
ingress:
  enabled: true
  className: nginx
  hosts:
    - host: flaresolverr.example.com
      paths:
        - path: /
          pathType: ImplementationSpecific
```

## Ports

| Port | Description |
|------|-------------|
| 8191 | FlareSolverr API |

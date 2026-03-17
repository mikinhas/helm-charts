# Jackett Helm Chart

Helm chart to deploy [Jackett](https://github.com/Jackett/Jackett) on Kubernetes.

Jackett works as a proxy server: it translates queries from apps (Sonarr, Radarr, etc.) into tracker-site-specific HTTP queries.

## Installation

```bash
helm repo add mikinhas https://mikinhas.github.io/helm-charts
helm install jackett mikinhas/jackett
```

## Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `replicaCount` | Number of replicas | `1` |
| `image.repository` | Docker image | `linuxserver/jackett` |
| `image.tag` | Image tag | `0.22.1393` |
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
  - name: UMASK
    value: "022"
```

### Ingress

```yaml
ingress:
  enabled: true
  className: nginx
  hosts:
    - host: jackett.example.com
      paths:
        - path: /
          pathType: ImplementationSpecific
```

## Ports

| Port | Description |
|------|-------------|
| 9117 | Jackett web UI |

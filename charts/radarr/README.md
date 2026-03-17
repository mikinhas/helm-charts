# Radarr Helm Chart

Helm chart to deploy [Radarr](https://github.com/Radarr/Radarr) on Kubernetes.

Radarr is a movie collection manager for Usenet and BitTorrent users. It monitors RSS feeds and automatically searches and downloads movies.

## Installation

```bash
helm repo add mikinhas https://mikinhas.github.io/helm-charts
helm install radarr mikinhas/radarr
```

## Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `replicaCount` | Number of replicas | `1` |
| `image.repository` | Docker image | `linuxserver/radarr` |
| `image.tag` | Image tag | `5.17.2` |
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
    - host: radarr.example.com
      paths:
        - path: /
          pathType: ImplementationSpecific
```

## Ports

| Port | Description |
|------|-------------|
| 7878 | Radarr web UI |

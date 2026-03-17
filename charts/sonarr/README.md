# Sonarr Helm Chart

Helm chart to deploy [Sonarr](https://github.com/Sonarr/Sonarr) on Kubernetes.

Sonarr is a TV series collection manager for Usenet and BitTorrent users. It monitors RSS feeds and automatically searches and downloads episodes.

## Installation

```bash
helm repo add mikinhas https://mikinhas.github.io/helm-charts
helm install sonarr mikinhas/sonarr
```

## Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `replicaCount` | Number of replicas | `1` |
| `image.repository` | Docker image | `linuxserver/sonarr` |
| `image.tag` | Image tag | `4.0.13` |
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
    - host: sonarr.example.com
      paths:
        - path: /
          pathType: ImplementationSpecific
```

## Ports

| Port | Description |
|------|-------------|
| 8989 | Sonarr web UI |

# Jellyseerr Helm Chart

Helm chart to deploy [Jellyseerr](https://github.com/fallenbagel/jellyseerr) on Kubernetes.

Jellyseerr is a request management and media discovery tool for Jellyfin, Plex and Emby:
users request movies and series, requests are forwarded to Radarr and Sonarr.
Configuration lives in `/app/config`.

## Installation

```bash
helm repo add mikinhas https://mikinhas.github.io/helm-charts
helm install jellyseerr mikinhas/jellyseerr
```

## Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `replicaCount` | Number of replicas | `1` |
| `image.repository` | Docker image | `fallenbagel/jellyseerr` |
| `image.tag` | Image tag | `2.7.3` |
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
  - name: TZ
    value: "Europe/Paris"
  - name: LOG_LEVEL
    value: "info"
```

### Persistence

```yaml
persistence:
  enabled: true
  volumes:
    - name: config
      size: 1Gi
volumes:
  - name: jellyseerr-config
    persistentVolumeClaim:
      claimName: jellyseerr-config
volumeMounts:
  - name: jellyseerr-config
    mountPath: /app/config
```

## Ports

| Port | Description |
|------|-------------|
| 5055 | Jellyseerr web UI |

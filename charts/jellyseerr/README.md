# Jellyseerr Helm Chart

Helm chart to deploy [Seerr](https://github.com/seerr-team/seerr) (formerly Jellyseerr) on Kubernetes.

Seerr is a request management and media discovery tool for Jellyfin, Plex and Emby:
users request movies and series, requests are forwarded to Radarr and Sonarr.
Configuration lives in `/app/config`. The container runs as user `node` (1000:1000), so that
directory must be writable by UID 1000 (`fsGroup: 1000` is set by default; on storage classes that
ignore it, such as local-path, `chown -R 1000:1000` the directory once).

Seerr 3.x is required for Jellyfin 10.11+ / 12: older Jellyseerr releases fail to authenticate
(`ArgumentNullException: request.App`).

## Installation

```bash
helm repo add mikinhas https://mikinhas.github.io/helm-charts
helm install jellyseerr mikinhas/jellyseerr
```

## Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `replicaCount` | Number of replicas | `1` |
| `image.repository` | Docker image | `ghcr.io/seerr-team/seerr` |
| `image.tag` | Image tag | `v3.5.1` |
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

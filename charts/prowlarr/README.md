# Prowlarr Helm Chart

Helm chart to deploy [Prowlarr](https://github.com/Prowlarr/Prowlarr) on Kubernetes.

Prowlarr is an indexer manager/proxy built on the *arr stack: it manages your torrent trackers
and Usenet indexers in one place and syncs them to Sonarr, Radarr, Lidarr, Readarr, etc.
It supports [FlareSolverr](https://github.com/FlareSolverr/FlareSolverr) as an indexer proxy.

## Installation

```bash
helm repo add mikinhas https://mikinhas.github.io/helm-charts
helm install prowlarr mikinhas/prowlarr
```

## Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `replicaCount` | Number of replicas | `1` |
| `image.repository` | Docker image | `linuxserver/prowlarr` |
| `image.tag` | Image tag | `2.7.0` |
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
    - host: prowlarr.example.com
      paths:
        - path: /
          pathType: ImplementationSpecific
```

## Ports

| Port | Description |
|------|-------------|
| 9696 | Prowlarr web UI |

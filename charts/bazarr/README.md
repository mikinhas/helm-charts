# Bazarr Helm Chart

Helm chart to deploy [Bazarr](https://github.com/morpheus65535/bazarr) on Kubernetes.

Bazarr is a companion application to Sonarr and Radarr: it manages and downloads subtitles
based on your requirements. Mount your media at the same path as in Sonarr/Radarr.

## Installation

```bash
helm repo add mikinhas https://mikinhas.github.io/helm-charts
helm install bazarr mikinhas/bazarr
```

## Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `replicaCount` | Number of replicas | `1` |
| `image.repository` | Docker image | `linuxserver/bazarr` |
| `image.tag` | Image tag | `1.6.2` |
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
    - host: bazarr.example.com
      paths:
        - path: /
          pathType: ImplementationSpecific
```

## Ports

| Port | Description |
|------|-------------|
| 6767 | Bazarr web UI (health check: `/api/system/ping`) |

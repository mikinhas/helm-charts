# Transmission Helm Chart

Helm chart to deploy [Transmission](https://transmissionbt.com/) on Kubernetes.

Transmission is a fast, easy, and free open-source BitTorrent client.

## Installation

```bash
helm repo add mikinhas https://mikinhas.github.io/helm-charts
helm install transmission mikinhas/transmission
```

## Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `replicaCount` | Number of replicas | `1` |
| `image.repository` | Docker image | `linuxserver/transmission` |
| `image.tag` | Image tag | `4.0.6` |
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
  - name: TRANSMISSION_WEB_HOME
    value: ""
  - name: USER
    value: ""
  - name: PASS
    value: ""
  - name: PEERPORT
    value: ""
```

### Persistence

```yaml
persistence:
  enabled: true
  volumes:
    - name: downloads
      size: 10Gi
    - name: config
      size: 1Gi
    - name: watch
      size: 1Gi
```

### Ingress

```yaml
ingress:
  enabled: true
  className: nginx
  hosts:
    - host: transmission.example.com
      paths:
        - path: /
          pathType: ImplementationSpecific
```

## Ports

| Port | Description |
|------|-------------|
| 9091 | Transmission web UI |

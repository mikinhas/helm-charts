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
| `peerService.enabled` | Enable the peer service (TCP/UDP) | `false` |
| `peerService.type` | Peer service type | `ClusterIP` |
| `peerService.port` | Peer service port | `51413` |
| `peerService.nodePort` | Peer service node port | `""` |
| `peerService.externalTrafficPolicy` | Peer service external traffic policy | `""` |
| `peerService.annotations` | Peer service annotations | `{}` |
| `ingress.enabled` | Enable ingress | `false` |
| `persistence.enabled` | Enable persistence | `false` |
| `resources` | CPU/Memory resource limits | `{}` |
| `podEnvVars` | Environment variables | `[]` |
| `config.enabled` | Manage `settings.json` from the chart | `true` |
| `config.settings` | Content of `settings.json` (YAML, rendered to JSON) | `{}` |

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

### Transmission settings

Transmission's `settings.json` is managed by the chart. The settings are rendered into a
ConfigMap and copied to `/config/settings.json` by an init container every time the pod
starts, so values set here always win over changes made from the web UI. Keys that are
not set keep Transmission's defaults. The `USER`, `PASS`, `PEERPORT`, `WHITELIST` and
`HOST_WHITELIST` environment variables are still applied on top of this file by the
image at startup.

The chart ships an empty `settings` map, so every setting takes Transmission's built-in
default. Note that the built-in `download-dir` is `/config/Downloads`, not
`/downloads/complete` as in the image, so you will usually want to set at least the
directories:

```yaml
config:
  settings:
    download-dir: /data/downloads/complete
    incomplete-dir: /data/downloads/incomplete
    speed-limit-down: 5000
    speed-limit-down-enabled: true
    ratio-limit: 2
    ratio-limit-enabled: true
```

Set `config.enabled: false` to let Transmission manage the file itself. The file is
copied into the volume mounted at `/config`. If there is none, the chart adds an
`emptyDir` for it. See the
[Transmission documentation](https://github.com/transmission/transmission/blob/main/docs/Editing-Configuration-Files.md)
for the list of settings.

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
| 51413 | BitTorrent peer port (TCP/UDP) |

### Peer service

The peer port is exposed by a dedicated service, disabled by default:

```yaml
peerService:
  enabled: true
  type: LoadBalancer
```

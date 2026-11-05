# Recyclarr Helm Chart

Helm chart to deploy [Recyclarr](https://github.com/recyclarr/recyclarr) on Kubernetes.

Recyclarr syncs the quality profiles, quality definitions and custom formats maintained by the
[TRaSH Guides](https://trash-guides.info/) into Sonarr and Radarr, so release selection follows
well-maintained rules instead of the stock defaults.

Recyclarr is a one-shot CLI. This chart runs `recyclarr sync` as a **CronJob** rather than
keeping the image's built-in cron daemon resident between syncs.

## Installation

```bash
helm repo add mikinhas https://mikinhas.github.io/helm-charts
helm install recyclarr mikinhas/recyclarr
```

## Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `image.repository` | Docker image | `ghcr.io/recyclarr/recyclarr` |
| `image.tag` | Image tag | `8.8.1` |
| `image.pullPolicy` | Pull policy | `IfNotPresent` |
| `schedule` | Cron schedule | `0 4 * * *` |
| `timeZone` | Time zone the schedule is evaluated in | `Europe/Paris` |
| `concurrencyPolicy` | CronJob concurrency policy | `Forbid` |
| `suspend` | Keep the CronJob but never run it | `false` |
| `args` | Arguments passed to the `recyclarr` CLI | `[sync]` |
| `config` | Contents of `recyclarr.yml` (string, rendered with `tpl`) | commented example |
| `secrets` | Contents of `secrets.yml` (string), for `!secret` references | `""` |
| `envFrom` | Extra env sources, e.g. a Secret holding API keys | `[]` |
| `podEnvVars` | Environment variables | `TZ=Europe/Paris` |
| `persistence.enabled` | Keep Recyclarr's data dir (git clones, logs) between runs | `false` |
| `resources` | CPU/Memory resource limits | `{}` |

### Configuration file

`config` is kept as a string so that Recyclarr's YAML tags (`!env_var`, `!secret`) are passed
through untouched. API keys are best injected from a Secret created outside the chart:

```bash
kubectl create secret generic recyclarr \
  --from-literal=SONARR_API_KEY=... \
  --from-literal=RADARR_API_KEY=...
```

```yaml
envFrom:
  - secretRef:
      name: recyclarr

config: |
  sonarr:
    sonarr:
      base_url: http://sonarr:80
      api_key: !env_var SONARR_API_KEY
      quality_definition:
        type: series
      quality_profiles:
        - trash_id: 58e8dc75731040612dd6f4ac0676bfd7  # [French MULTi.VF] HD Bluray + WEB (1080p)
          reset_unmatched_scores:
            enabled: true
      custom_format_groups:
        add:
          - trash_id: 158188097a58d7687dee647e04af0da3  # [Optional] Golden Rule HD
          - trash_id: a23c8675c79118544fd74153394fa589  # [Unwanted] Unwanted Formats French
```

Ready-made configurations for every TRaSH profile (1080p / 2160p, VO / VF / VOSTFR, anime,
German...) live in the [config-templates](https://github.com/recyclarr/config-templates)
repository: copy one under `config` and replace `base_url` / `api_key`.

### Preview mode

Set `args: [sync, --preview]` to log what would change without touching Sonarr or Radarr.

### Running a sync now

```bash
kubectl create job --from=cronjob/recyclarr recyclarr-manual-$(date +%s)
```

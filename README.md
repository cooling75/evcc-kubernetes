# evcc-kubernetes

![GitHub Release Date](https://img.shields.io/github/release-date/cooling75/evcc-kubernetes) 
 ![Dynamic YAML Badge](https://img.shields.io/badge/dynamic/yaml?url=https%3A%2F%2Fraw.githubusercontent.com%2Fcooling75%2Fevcc-kubernetes%2Frefs%2Fheads%2Fmain%2Fcharts%2Fevcc-kubernetes%2FChart.yaml&query=%24.version&label=AppVersion)
 ![Dynamic YAML Badge](https://img.shields.io/badge/dynamic/yaml?url=https%3A%2F%2Fraw.githubusercontent.com%2Fcooling75%2Fevcc-kubernetes%2Frefs%2Fheads%2Fmain%2Fcharts%2Fevcc-kubernetes%2FChart.yaml&query=%24.appVersion&label=AppVersion)
 ![GitHub Release](https://img.shields.io/github/v/release/cooling75/evcc-kubernetes)


Installs [evcc](https://evcc.io/) to Kubernetes via helm chart.

## Installation

```bash
helm repo add evcc-kubernetes https://cooling75.github.io/evcc-kubernetes/ 

helm install evcc-release evcc-kubernetes/evcc-kubernetes --values values.yaml
```

# Configuration

By default evcc starts without a config file and is set up via its web
interface, as recommended by the [evcc docs](https://docs.evcc.io/en/installation/configuration).
On the first visit you will be asked to set an admin password. Everything you
set up there is stored in the evcc database (see [Data and storage](#data-and-storage)).

Alternatively, put your evcc configuration under `evcc:` in your values file.
It is rendered as `/etc/evcc.yaml` exactly as given, and the pod restarts
automatically when it changes:

```yaml
evcc:
  network:
    host: evcc.local
  mqtt:
    broker: mosquitto.local
    topic: evcc
```

A complete example with simulated devices is in
[`charts/evcc-kubernetes/ci/demo-values.yaml`](charts/evcc-kubernetes/ci/demo-values.yaml).
To check an evcc.yaml for YAML errors and unknown keys before installing it,
run (device settings are only checked when evcc starts, see the pod log):

```bash
docker run --rm --entrypoint evcc -v "$(pwd)/evcc.yaml:/etc/evcc.yaml:ro" evcc/evcc:latest checkconfig
```

# Data and storage

The evcc database in `/root/.evcc` holds your charging sessions and everything
set up in the web interface. The chart stores it on a PersistentVolumeClaim
(`persistence.enabled: true`, 500Mi by default), so your cluster needs a default
StorageClass (k3s ships `local-path`). Otherwise set `persistence.storageClass`,
or use a PVC of your own with `persistence.existingClaim`.

evcc uses SQLite, which does not work reliably on NFS. Prefer local storage or
a block storage provider such as Longhorn.

# Upgrading to 1.8.0

1.8.0 changes the defaults for setups at home:

- **No demo configuration by default.** Older versions shipped a demo setup in
  `evcc:` that was merged into your own values. If you relied on parts of it
  (for example `site` or `loadpoints`), add them to your values file. Without
  any `evcc:` values, evcc now starts in web interface mode.
- **Persistence is enabled by default.** If you really want no persistence, set
  `persistence.enabled: false`.
- **Log level `info`** (evcc's default) instead of `debug`, which was part of
  the demo configuration.
- **Rollouts stop the old pod first** (`RollingUpdate` with `maxSurge: 0`), so
  evcc never runs twice. This replaces `Recreate`, which older versions used
  with persistence enabled.

# Uninstall

```bash
helm uninstall evcc-release
```

The PVC with the evcc database is kept, so a reinstall with the same release
name picks up your data again. To delete the data as well:

```bash
kubectl delete pvc -l app.kubernetes.io/instance=evcc-release
```

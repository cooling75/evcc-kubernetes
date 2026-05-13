# evcc-kubernetes

Installs evcc to Kubernetes via helm chart.

## Installation

```bash
helm repo add evcc-kubernetes https://cooling75.github.io/helm-charts

helm install my-evcc-release evcc-kubernetes/evcc-kuernetes
```

# Uninstall

```bash
helm uninstall evcc-release
``` 
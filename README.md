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
To configure your evcc instance you can use the web using the GUI or add your
desired values to the values.yaml and install with '--values values-file.yaml' flag.

The easiest way to create your values is to run a docker container:
```
# Create evcc configuration file
touch evcc.yaml         
# Run evcc in docker with mapping to configuration file
docker run -v $(pwd)/evcc.yaml:/app/evcc.yaml -it evcc/evcc:latest evcc configure 
```

Copy the values from evcc.yaml to this charts vaules file e.g. by replacing the demo parameters.

# Uninstall

```bash
helm uninstall evcc-release
``` 

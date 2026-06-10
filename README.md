# evcc-kubernetes

Installs evcc to Kubernetes via helm chart.

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

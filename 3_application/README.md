# Deploy MPI application

First build the container with our sample pingpong benchmark:

```
podman build -f Dockerfile.pingpong -t ntosato/mpi-container:pingpong
```

## Install MPI-operator

Deploy the MPI-operator:

```
kubectl apply -f https://raw.githubusercontent.com/kubeflow/mpi-operator/v0.4.0/deploy/v2beta1/mpi-operator.yaml
```


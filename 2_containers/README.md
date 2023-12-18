# Containers

The workflow employ at leat 3 different containers:

- A builder container to compile the MPI application
- A runner container to run the MPI application
- A base container common to the containers above

## Build the common container:

```
podman build -f Dockerfile -t mpi-container:base
```

## Builder container

```
podman build -f openmpi-builder.Dockerfile -t mpi-container:builder
```

## Build runner

```
podman build -f openmpi.Dockerfile -t mpi-container:runner
```






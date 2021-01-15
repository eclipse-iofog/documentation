# Overview

The ioFog Kubernetes integration allows user to manage both cloud and edge workloads via `kubectl` and `iofogctl`.

# Use Cases

The following use cases are achieved through the following Custom Resources and corresponding Custom Controllers (in Operator):

* Control Plane
* Agent
* Application
* Microservice
* Route
* Edge Resource


#### Manage Control Plane

```
kubectl apply -f controlplane.yaml
kubectl create -f controlplane.yaml
kubectl edit controlplane NAME
kubectl describe controlplane NAME
kubectl get controlplanes
kubectl delete controlplane NAME
...
```

#### Manage Agents

*Agents cannot be created / deleted via kubectl as this would involve Operator SSHing into Agents.*

```
kubectl edit agent NAME
kubectl describe agent NAME
kubectl get agent
...
```

#### Manage Applications

```
kubectl apply -f app.yaml
kubectl create -f app.yaml
kubectl edit application NAME
kubectl describe application NAME
kubectl get applications
kubectl delete application NAME
...
```

#### Manage Microservices

```
kubectl apply -f msvc.yaml
kubectl create -f msvc.yaml
kubectl edit microservice NAME
kubectl describe microservice NAME
kubectl get microservices
kubectl delete microservices NAME
...
```

#### Manage Routes

```
kubectl apply -f route.yaml
kubectl create -f route.yaml
kubectl edit route NAME
kubectl describe route NAME
kubectl get routes
kubectl delete routes NAME
...
```

#### Manage Edge Resources

```
kubectl apply -f edge-resource.yaml
kubectl create -f edge-resource.yaml
kubectl edit edge-resource NAME
kubectl describe edge-resource NAME
kubectl get edge-resources
kubectl delete edge-resources NAME
...
```

# Requirements

The ioFog Kubernetes integration...
1. **MUST** allow iofogctl and kubectl to use the same YAML specs for all resources.
2. **MUST** allow Applications and Microservices to run on both the cloud and the edge.

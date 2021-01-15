# Overview

The ioFog Kubernetes integration allows user to manage both cloud and edge workloads via `kubectl` and `iofogctl`.

# Use Cases

The following use cases are achieved through these Custom Resources and their corresponding Custom Controllers (in Operator):

* Control Plane
* Agent
* Application
* Microservice
* Route
* Edge Resource


#### 1. Manage Control Plane

The Control Plane is a set of ioFog components that run on the Kubernetes cluster as containers. These include the Controllers and the Port Manager.

```
kubectl apply -f controlplane.yaml
kubectl create -f controlplane.yaml
kubectl edit controlplane NAME
kubectl describe controlplane NAME
kubectl get controlplanes
kubectl delete controlplane NAME
...
```

#### 2. Manage Agents

Agents are edge devices running the ioFog Agent stack. 

Agents cannot be created / deleted via `kubectl` as this would involve the Operator SSHing into remote hosts to install/uninstall the ioFog Agent stack.

```
kubectl edit agent NAME
kubectl describe agent NAME
kubectl get agent
...
```

#### 3. Manage Applications

Applications are a set of Microservices that can be run on either the cloud or the edge, or both.

```
kubectl apply -f app.yaml
kubectl create -f app.yaml
kubectl edit application NAME
kubectl describe application NAME
kubectl get applications
kubectl delete application NAME
...
```

#### 4. Manage Microservices

Microservices are individual workloads which run as containers on either the cloud or the edge, or both.

```
kubectl apply -f msvc.yaml
kubectl create -f msvc.yaml
kubectl edit microservice NAME
kubectl describe microservice NAME
kubectl get microservices
kubectl delete microservices NAME
...
```

#### 5. Manage Routes

Routes are a unidirectional channels which allow Microservices to reach each other through ioMessages.

```
kubectl apply -f route.yaml
kubectl create -f route.yaml
kubectl edit route NAME
kubectl describe route NAME
kubectl get routes
kubectl delete routes NAME
...
```

#### 6. Manage Edge Resources

Edge Resources represent arbitrary, user-defined software and devices running on Agents.

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

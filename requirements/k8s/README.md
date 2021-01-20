
# Contents

######[1. Overview](#1-overview)
######[2. Use Cases](#2-use-cases)
######[3. Requirements](#3-requirements)

# 1. Overview

ioFog provides an integration with Kubernetes so as to allow users to manage cloud and edge workloads through `kubectl`.

The integration is implemented through the following Custom Resources and their corresponding Custom Controllers (in ioFog Operator):

* Control Plane
* Agent
* Application
* Microservice
* Route
* Edge Resource

# 2. Use Cases

#### 2.1 Manage Control Plane

The Control Plane is a set of ioFog components that run on the Kubernetes cluster as containers. These include the Controllers, the Router and the Port Manager.

```
kubectl apply -f controlplane.yaml
kubectl edit controlplane NAME
kubectl describe controlplane NAME
kubectl get controlplanes
kubectl delete controlplane NAME
...
```

#### 2.2 Manage Agents

Agents are edge devices running the ioFog Agent stack. 

Agent creation / deletion would involve some container on the Kubernetes cluster SSHing into the edge hosts to install / uninstall the Agent stack. This could be done directly by ioFog Operator or through a Job that ioFog Operator creates. Either way, it would be up to the users to ensure that a container on the cluster is able to reach the edge devices via SSH.

```
kubectl apply -f agent.yaml
kubectl create -f agent.yaml
kubectl edit agent NAME
kubectl describe agent NAME
kubectl get agent
...
```

#### 2.3 Manage Applications

Applications are a set of Microservices that can be run on the edge.

```
kubectl apply -f app.yaml
kubectl create -f app.yaml
kubectl edit application NAME
kubectl describe application NAME
kubectl get applications
kubectl delete application NAME
...
```

#### 2.4 Manage Microservices

Microservices are individual workloads which run as containers on the edge.

```
kubectl apply -f msvc.yaml
kubectl create -f msvc.yaml
kubectl edit microservice NAME
kubectl describe microservice NAME
kubectl get microservices
kubectl delete microservices NAME
...
```

#### 2.5 Manage Routes

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

#### 2.6 Manage Edge Resources

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

# 3. Requirements

The ioFog Kubernetes integration...

3.1 **MUST** allow `iofogctl` and `kubectl` to use the same YAML specs for all resources.
3.2 ...

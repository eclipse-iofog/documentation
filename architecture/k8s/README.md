
# Contents

[1. Overview](#1-overview)

[2. ioFog Operator](#2-iofog-operator)

[3. Custom Resources](#3-custom-resources)

[4. Routers](#4-routers)

[5. Port Manager](#5-port-manager)

# 1. Overview

An ioFog Edge Compute Network ('ECN') consists of a Control Plane, a set of Agents, Routers, and user-defined Applications.

For production environments, the ioFog Control Plane is typically deployed on a Kubernetes cluster.

![](https://github.com/eclipse-iofog/documentation/blob/master/architecture/k8s/assets/overview.png?raw=true)

Users can manage both their ECNs through `iofogctl` or `kubectl`. This capability is provided through a set of Custom Resources and corresponding Custom Controllers which run inside the ioFog Operator.

# 2. ioFog Operator

The [iofog Operator](https://github.com/eclipse-iofog/iofog-operator) manages Custom Controllers for all ioFog Custom Resources. Users can make declarative changes to Custom Resources and the Operator will reconcile the differences between the current state of the ECN and the desired state as required.

## Deployment

The Operator can be deployed to a Kubernetes namespace via [helm](https://iofog.org/docs/2/platform-deployment/kubernetes-helm.html) or [iofogctl](https://iofog.org/docs/2/platform-deployment/kubernetes-iofogctl.html).

The Operator runs in a namespace-scoped mode. As such, a single Operator instance is only ever responsible for a single ECN. Note that, if using `iofogctl`, there is a 1-to-1 mapping between the `iofogctl` namespace and the Kubernetes namespace within which the Operator resides.

#### CRDs

The deployment process will create the following Custom Resource Definitions:

* Control Planes
* Agents
* Applications
* Microservices
* Public Ports
* Routes
* Edge Resources

#### RBAC

The RBAC resources created alongside the ioFog Operator during deployment can be found [here](https://github.com/eclipse-iofog/iofog-operator/tree/develop/config).

# 3. Custom Resources

Upon deployment of the ioFog Operator, users will be able to use `kubectl` to create and modify a number Custom Resources for the purposes of managing the Control Plane, Agents, and edge workloads.

## Control Planes

The ioFog Control Plane consists of one or more [Controllers](https://github.com/eclipse-iofog/controller), a [Port Manager](https://github.com/eclipse-iofog/port-manager), and a [Router](https://github.com/eclipse-iofog/router).

All components of the Control Plane are deployed as Kubernetes Deployments. Users can modify replica counts of the Controller and Router Deployments for high availability. The Port Manager runs a leader election algorithm to ensure only a single instance is operating at any one time. Nonetheless, Port Manager replicas can be increased for failover.

Controllers and Routers have public APIs which can be exposed by `LoadBalancer` Services or `ClusterIP` services with separate `Ingress` configured.

The Control Plane CR YAML specification can be found [here](https://github.com/eclipse-iofog/iofog-operator/blob/develop/config/cr/controlplane.yaml).

## Agents

Agents are edge hosts that are running the ioFog Agent stack. The Agent Custom Resource can be used to connect an existing Agent to the corresponding ECN or to deploy a new Agent altogether and install the ioFog stack on a remote host.

When deploying a new Agent, the Operator is responsible for installing the ioFog Agent stack on a remote host via SSH. As such, users must ensure that the cluster network and the remote host network allow for the required SSH connection.

The Agent CR YAML specification will be available when it is supported.

## Applications

Applications are a group of Microservices that can be managed together.

The Application CR YAML specification will be available when it is supported.

## Microservices

Microservices are definitions of a container and associated resources for the purposes of orchestrating containers on the edge.

The Microservice CR YAML specification will be available when it is supported.

###### Public Ports

Public Ports describe externally accessible endpoints for Microservices. They are facilitated by the ECN's Routers and Port Manager.

Public Ports allow bi-directional traffic to Microservices at the edge without the need to create firewall rules for inbound traffic in the edge network.

The Public Port CR YAML specification will be available when it is supported.

## Routes

Routes are a unidirectional communication channels which allow Microservices to reach each other through ioMessages.

The Route CR YAML specification will be available when it is supported.

## Edge Resources

Edge Resources represent arbitrary, user-defined software and devices running on Agents.

The Edge Resource CR YAML specification will be available when it is supported.

# 4. Routers

Routers are components of an Edge Compute Network responsible for establishing uni-directional communication links between Microservices.

Each Control Plane contains an Interior Router. When an Agent is deployed to the ECN, the Agent's Edge Router connects to the Interior Router. This connection allows users to access Microservice endpoints running on Agents without configuring inbound network traffic rules to the network the Agents reside in. This is because the Edge router establishes a bi-directional connection with the Interior Router.

# 5. Port Manager

The Port Manager is a component of the Control Plane which is responsible for managing Kubernetes Services for the purposes of exposing Microservice endpoints to users.

When a user creates a Public Port for a Microservice, the Port Manager becomes aware of this and creates the requisite Kubernetes Services to enable external access.

The Port Manager can be configured to make `LoadBalancer` Services or `ClusterIP` Services with a separate `Ingress` configured.

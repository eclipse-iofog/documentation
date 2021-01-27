
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

The following is the Control Plane CR YAML spec:

```YAML
apiVersion: iofog.org/v2
kind: ControlPlane
metadata:
  namespace: default
  name: iofog
spec:

# Required
  user:
    name: Serge
    surname: Radinovich
    email: serge@edgeworx.io
    pwSecret: iofog-cr-pw
  replicas:
    controller: 1

# Optional
  services:
    controller:
      type: LoadBalancer
      address: ""
    router:
      type: LoadBalancer
      address: ""
    proxy:
      type: LoadBalancer
      address: ""
  database:
    provider: ""
    host: ""
    port: 0
    user: ""
    password: ""
    databaseName: ""
  images:
    pullSecret: ""
    kubelet: ""
    controller: ""
    router: ""
    portManager: ""
    proxy: ""
  ingresses:
    router:
      address: ""
      httpPort: 0
      messagePort: 0
      interiorPort: 0
      edgePort: 0
    httpProxy:
      address: ""
    tcpProxy:
      address: ""
  controller:
    pidBaseDir: ""
    ecnViewerPort: 0
```

## Agents

Agents are edge hosts that are running the ioFog Agent stack. The Agent Custom Resource can be used to connect an existing Agent to the corresponding ECN or to deploy a new Agent altogether and install the ioFog stack on a remote host.

When deploying a new Agent, the Operator is responsible for installing the ioFog Agent stack on a remote host via SSH. As such, users must ensure that the cluster network and the remote host network allow for the required SSH connection.

The following is the Agent CR YAML spec:

```YAML
apiVersion: iofog.org/v2
kind: Agent
metadata:
  namespace: default
  name: meerkat
spec:

# Required
  host: 30.40.50.6
  ssh:
    user: foo
    keySecret: agent-meerkat-key
    port: 22

# Optional
  config:
    description: agent running on VM
    latitude: 46.204391
    longitude: 6.143158
    agentType: auto
    dockerUrl: unix:///var/run/docker.sock
    diskLimit: 50
    diskDirectory: /var/lib/iofog-agent/
    memoryLimit: 4096
    cpuLimit: 80
    logLimit: 10
    logDirectory: /var/log/iofog-agent/
    logFileCount: 10
    statusFrequency: 10
    changeFrequency: 10
    deviceScanFrequency: 60
    bluetoothEnabled: true
    watchdogEnabled: false
    abstractedHardwareEnabled: false
    upstreamRouters: ['default-router']
    networkRouter: ''
    routerConfig:
      routerMode: edge
      messagingPort: 5672
      edgeRouterPort: 56721
      interRouterPort: 56722
    dockerPruningFrequency: 1
    logLevel: INFO
    availableDiskThreshold: 90
  scripts:
    dir: /agent-scripts
    deps:
      entrypoint: install_deps.sh
    install:
      entrypoint: install_iofog.sh
      args:
        - 2.0.1
    uninstall:
      entrypoint: uninstall_iofog.sh
```

## Applications

Applications are a group of Microservices that can be managed together.

The following is the Application CR YAML spec:

```YAML
apiVersion: iofog.org/v2
kind: Application
metadata:
  name: health-care-wearable
  namespace: default
spec:
  microservices:
    - name: heart-rate-monitor
      agent:
        name: horse-1
      images:
        arm: edgeworx/healthcare-heart-rate:arm-v1
        x86: edgeworx/healthcare-heart-rate:x86-v1
      container:
        rootHostAccess: false
        ports: []
      config:
        test_mode: true
        data_label: Anonymous Person
        nested_object:
          key: 42
          deep_nested:
            foo: bar
    - name: heart-rate-viewer
      agent:
        name: horse-1
      images:
        arm: edgeworx/healthcare-heart-rate-ui:arm
        x86: edgeworx/healthcare-heart-rate-ui:x86
        registry: remote
      container:
        rootHostAccess: false
        ports:
          - internal: 80
            external: 5000
            public: 5001
            protocol: tcp
        env:
          - key: BASE_URL
            value: http://localhost:8080/data
      config:
        test: 54
```

## Microservices

Microservices are definitions of a container and associated resources for the purposes of orchestrating containers on the edge.

The following is the Microservice CR YAML spec:

```YAML
apiVersion: iofog.org/v2
kind: Microservice
metadata:
  name: heart-rate-monitor
  namespace: default
spec:
  application: Healthcare Wearable
  agent:
    name: zebra-1
  images:
    x86: edgeworx/healthcare-heart-rate:x86-v1
    arm: edgeworx/healthcare-heart-rate:arm-v1
    registry: remote
    catalogId: 0
  container:
    rootHostAccess: false
    volumes:
      - hostDestination: /tmp/msvc
        containerDestination: /data
        accessMode: 'rw'
        type: 'bind'
    env:
      - key: BASE_URL
        value: http://localhost:8080/data
    ports:
      - internal: 80
        external: 5000
        public: 5001
        host: default-router
        protocol: http
    commands:
      - 'dbhost'
      - 'localhost:27017'
  config:
    data_label: test_mode=false_cross_agent_microservice_routing_aug_27
    test_mode: true
  rebuild: false
```

###### Public Ports

Public Ports describe externally accessible endpoints for Microservices. They are facilitated by the ECN's Routers and Port Manager.

Public Ports allow bi-directional traffic to Microservices at the edge without the need to create firewall rules for inbound traffic in the edge network.

The Microservice CR YAML specification will be available when it is supported.

## Routes

Routes are a unidirectional communication channels which allow Microservices to reach each other through ioMessages.

The following is the Route CR YAML spec:

```YAML
apiVersion: iofog.org/v2
kind: Route
metadata:
  name: my-route
spec:
  from: frontend-msvc
  to: backend-msvc
```

## Edge Resources

Edge Resources represent arbitrary, user-defined software and devices running on Agents.

The following is the Edge Resource CR YAML spec:

```YAML
apiVersion: iofog.org/v2
kind: EdgeResource
metadata:
  name: smart-door
  namespace: default
spec:
  name: smart-door
  version: v1.0.0
  description: Very smart door
  interfaceProtocol: https
  interface:
    endpoints:
      - name: open
        method: PUT
        url: /open
      - name: close
        method: PUT
        url: /close
      - name: destroy
        method: DELETE
        url: /endOfTheWorld
  display:
    name: Smart Door
    icon: accessible-forward
    color: rgb(90, 200, 250)
  custom: {}
```

# 4. Routers

Routers are components of an Edge Compute Network responsible for establishing uni-directional communication links between Microservices.

Each Control Plane contains an Interior Router. When an Agent is deployed to the ECN, the Agent's Edge Router connects to the Interior Router. This connection allows users to access Microservice endpoints running on Agents without configuring inbound network traffic rules to the network the Agents reside in. This is because the Edge router establishes a bi-directional connection with the Interior Router.

# 5. Port Manager

The Port Manager is a component of the Control Plane which is responsible for managing Kubernetes Services for the purposes of exposing Microservice endpoints to users.

When a user creates a Public Port for a Microservice, the Port Manager becomes aware of this and creates the requisite Kubernetes Services to enable external access.

The Port Manager can be configured to make `LoadBalancer` Services or `ClusterIP` Services with a separate `Ingress` configured.

# Eclipse ioFog Current Roadmap

## Version 2.0.0
### Edge Compute Networks
- Allow Docker networks to be created on edge nodes using Controller commands
- Retire Connector in favor of new Skupper-based ECN model
- Add MQTT, gRPC, and Websocket implementations of agent-to-controller APIs
- Multi-signature support for full data provenance wherein data cannot be opened without all registered signatures
- New model ECN with peering optimization, cost mechanism, dynamic routing, and true IP-layer passthrough

### EdgeOps
- General “edge attribute” registration databank in the control plane, allowing for buildout of any type of edge device information to flow through ioFog - this enables device management without becoming a device management platform
- Enhance logging functionality for edge microservices
- Finish enhancing status information about edge microservices (including leveraging the “healthy” status provided by Docker?)

### Orchestration
- Plug-and-play distribution of microservices upon detecting attached devices or hardware capabilities
- Standard data format for edge HW attributes (GPU, serial ports, etc.) that can be used to describe any compatible HW for each listed item
- Bootstrapping an edge node to join the k8s cluster or ECN from “right off the vendor shelf” condition with no privileged information loaded into the appliance

### Developer Experience
- Automated build of all possible edge microservice versions (arm32, arm64, gpu, vpu, etc.)

### Integrations

### Core Engine
- Add more control signals, such as low battery, security warning, etc.
- Strict control over docker containers on a node - whitelisting, cache fixes, etc.

## Q2 2020
### Edge Compute Networks
### EdgeOps
### Orchestration
### Developer Experience
### Integrations
### Core Engine

## H2 2020
### Edge Compute Networks
### EdgeOps
### Orchestration
### Developer Experience
### Integrations
### Core Engine

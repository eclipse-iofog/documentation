# Eclipse ioFog Current Roadmap

## Version 2.0.0
### Edge Compute Networks
- Allow Docker networks to be created on edge nodes using Controller commands
- Retire Connector in favor of new Skupper-based ECN model
- New model ECN with peering optimization, cost mechanism, dynamic routing, and true IP-layer passthrough

### EdgeOps
- Enhance logging functionality for edge microservices
- Finish enhancing status information about edge microservices (including leveraging the “healthy” status provided by Docker?)
- Universal logging capability with centralized access and distributed handling
- iofogctl for Windows OS
- Agent attach/detach functionality
- Microservice move functionality

### Orchestration
- Container tag format w/official slots for different attributes (arch, gpu, etc.)
- Expand x86/ARM architecture pattern to include any number of defined architectures and architecture+capability labels
- Develop a high-level edge application orchestration language or YAML file format that allows decisions to be made about where to move which microservices, etc. (Allocate microservices to agents with defined tags, as opposed to specific agent.)

### Developer Experience
- Automated build of all possible edge microservice versions (arm32, arm64, gpu, vpu, etc.)
- Better Minikube and local environment experience (show how this complements with the platform repo / tools)

### Integrations
- Red Hat project Skupper for new ECN model
- Eclipse Hono integration for standing up a Hono environment with edge capabilities from ioFog and key Hono components deployed and managed at the edge (Expose as tutorial series?, How to communicate the Hono integration?)
- VMware Tanzu integration (Octant first) for providing seamless edge capabilities for managed k8s environments

### Core Engine
- Add more control signals, such as low battery, security warning, etc.
- Finalize definition of agent-to-controller interface and APIs so we can roll out any number of agents and open the implementation to all (API definition should be complete for 2.0, but the process of developing your own agents doesn't have to be immediately in 2.0?)
- Add more interface and hardware abstractions… maybe adding an interface registry for looking up with microservice to run for which detected interface?
- Docker pruning in Agent
- Registry management in Controller

## Q2 2020
### Edge Compute Networks
- Edge Compute Network topology design and enactment through central control plane
- Security for accessing public ports so data access can be granted and revoked safely
- Add MQTT, gRPC, and Websocket implementations of agent-to-controller APIs

### EdgeOps
- Granular microservice usage tracking to allow for commercialization of edge microservices
- Peer sourcing of container images (bittorrent for microservice images)
- Finish updating catalog model and add commercialization capabilities
- Publish Edge Microservices Catalog (EMCat) data format for being able to host a marketplace anywhere
- Command to only pull edge microservices to an edge node but not yet start or finish the deployment - something like “staging” or “pull only”
- ECN Viewer UI enhancements

### Orchestration
- Standard data format for edge HW attributes (GPU, serial ports, etc.) that can be used to describe any compatible HW for each listed item
- Bootstrapping an edge node to join the k8s cluster or ECN from “right off the vendor shelf” condition with no privileged information loaded into the appliance
- Dependencies and timing and sequencing in starting up edge containers - similar to Docker Compose but with more control and universal so it works with or without Docker
- Kubernetes integration changing from virtual Kubelets to full use of Custom Resource Definitions
- Kubernetes full implementation of custom scheduling for ioFog CRDs and device CRDs exposed through ioFog

### Developer Experience
- Full-circle Edge AI development toolchain allowing training, model optimization, container inclusion, and deployment with low prior experience

### Integrations
- Eclipse Streamsheets integration for offering no-code development and operation of device and sensor data stream processing
- Harbor integration for management of container images, firmware binaries, and any bits
- VMware Tanzu integration (Mission Control second) for providing seamless edge capabilities for managed k8s environments

### Core Engine
- Deep detection of hardware capabilities for each edge node
- Identity and certificate management and lifecycle
- External “helpers” loaded by Agent to fix things on the host, detect location of CUDA, etc. automatically
- Finish implementing current security architecture concepts
- Strict control over docker containers on a node - whitelisting, cache fixes, etc.
- Enhanced management capabilities over edge nodes - maybe even OS updates, etc.?

## H2 2020
### Edge Compute Networks
- Multi-signature support for full data provenance wherein data cannot be opened without all registered signatures
- ECN topology visualization and management, allowing view of router roles and placement, view of traffic flow, and drag-and-drop topology editing

### EdgeOps
- General “edge attribute” registration databank in the control plane, allowing for buildout of any type of edge device information to flow through ioFog - this enables device management without becoming a device management platform
- Distributing a precious asset (an AI model) with access control for all allowed nodes in order to sequence updates

### Orchestration
- Plug-and-play distribution of microservices upon detecting attached devices or hardware capabilities
- Provide service mesh enhancements by using the control plane to understand where services have been stood up and inform nodes where to find them - a central service/api registry
- Service discovery

### Developer Experience
- IDE integrations
- Simulated edge environments (agents, devices, data streams, etc.) for building and testing

### Integrations
- Tangle integration for immutable, auditable edge ledger
- Edge FS integration for storage management from edge to cloud
- Eclipse Che for easier developer experience
- Spiffe integration for identities

### Core Engine
- Immutable, auditable ledger for edge events
- Divide accelerator hardware (VPU, TPU, GPU) to allow access by multiple microservices
- Time synchronization of edge nodes, potentially using GPS for the timestamp
- Implement event-driven system activities with audit logging, such as “pallet arrived, weight taken, bill of lading accepted”
- Define a master list of system events, such as “edge node joined ECN, security policy violation noted, etc.”
- Add multi-tenant functionality for the edge
- Add VMs, Firecracker VMs, and serverless and begin offering unikernel code in release version
- Add serverless functions as an edge microservice type for more constrained devices or just as an option for any hardware

## No Timeline
- dapr.io integration
- Harbor integration (image registry)
- Docker image sharing (use Bittorrent?)

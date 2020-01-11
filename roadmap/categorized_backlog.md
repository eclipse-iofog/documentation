# Categorized Backlog of Eclipse ioFog Enhancements
This document will likely disappear shortly. It is being used as a working doc for planning.

## Hardware Abstraction and Management
- Add more control signals, such as low battery, security warning, etc.
- Plug-and-play distribution of microservices upon detecting attached devices or hardware capabilities
- Add more interface and hardware abstractions… maybe adding an interface registry for looking up with microservice to run for which detected interface?
- Enhanced management capabilities over edge nodes - maybe even OS updates, etc.?
- Strict control over docker containers on a node - whitelisting, cache fixes, etc.
- Time synchronization of edge nodes, potentially using GPS for the timestamp
- External “helpers” loaded by Agent to fix things on the host, detect location of CUDA, etc. automatically
- Enhance logging functionality for edge microservices
- Peer sourcing of container images (bittorrent for microservice images)
- General “edge attribute” registration databank in the control plane, allowing for buildout of any type of edge device information to flow through ioFog - this enables device management without becoming a device management platform

## Communication and Networking
- Retire Connector in favor of new Skupper-based ECN model
- Add MQTT, gRPC, and Websocket implementations of agent-to-controller APIs
- Multi-signature support for full data provenance wherein data cannot be opened without all registered signatures
- New model ECN with peering optimization, cost mechanism, dynamic routing, and true IP-layer passthrough

## Kubernetes Integration
- Bootstrapping an edge node to join the k8s cluster or ECN from “right off the vendor shelf” condition with no privileged information loaded into the appliance

## CI/CD and Build Systems
- Automated build of all possible edge microservice versions (arm32, arm64, gpu, vpu, etc.)

## Edge Orchestration
- Granular microservice usage tracking to allow for commercialization of edge microservices
- Service discovery
- Command to only pull edge microservices to an edge node but not yet start or finish the deployment - something like “staging” or “pull only”
- Implement event-driven system activities with audit logging, such as “pallet arrived, weight taken, bill of lading accepted”
- Define a master list of system events, such as “edge node joined ECN, security policy violation noted, etc.”
- Provide service mesh enhancements by using the control plane to understand where services have been stood up and inform nodes where to find them - a central service/api registry
- Develop a high-level edge application orchestration language or YAML file format that allows decisions to be made about where to move which microservices, etc.
- Add multi-tenant functionality for the edge
- Container tag format w/official slots for different attributes (arch, gpu, etc.)
- Expand x86/ARM architecture pattern to include any number of defined architectures and architecture+capability labels
- Dependencies and timing and sequencing in starting up edge containers - similar to Docker Compose but with more control and universal so it works with or without Docker
- Finish updating catalog model and add commercialization capabilities
- Publish Edge Microservices Catalog (EMCat) data format for being able to host a marketplace anywhere

## Agent Improvements
- Finalize definition of agent-to-controller interface and APIs so we can roll out any number of agents and open the implementation to all
- Add VMs and serverless and begin offering unikernel code in release version
- Finish enhancing status information about edge microservices (including leveraging the “healthy” status provided by Docker?)
- Add serverless functions as an edge microservice type for more constrained devices or just as an option for any hardware
- Finish implementing current security architecture concepts
- Distributing a precious asset (an AI model) with access control for all allowed nodes in order to sequence updates

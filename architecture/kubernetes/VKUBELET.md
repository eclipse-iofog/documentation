# Virtual Kubelet

ioFog on Kubernetes was initially designed to leverage [Virtual Kubelet](https://github.com/virtual-kubelet/virtual-kubelet). Virtual Kubelet gave us access to the Kubernetes default scheduler and allowed ioFog concepts like Agents, Flows, and Microservices to appear as native Kubernetes types, namely Nodes, Deployments, and Pods. 

Ultimately, this approach did not give us the freedom required to enable all of the operational features users had available to them with ioFog without Kubernetes. As such, we have decided to move away from Virtual Kubelet and double down on our Operator and corresponding Custom Resources ('CRs').

The Operator currently manages two CRs: Applications and Kogs. Applications are a set of Microservices that are to be deployed on ioFog Agents. Kogs define the ioFog Control Plane and auxiliary resources which are required to be deployed on the Kubernetes cluster in order to support the larger Edge Compute Network ('ECN'). You can find details of these CRs [here](https://github.com/eclipse-iofog/iofog-operator/tree/develop/deploy/crds).

We plan to expand on the current Operator by creating new CRs and corresponding functionality.

## Todo List

* Remove Virtual Kubelet
* Update Operator to no longer create Kubernetes Deployments when an Application CR Definition is created.

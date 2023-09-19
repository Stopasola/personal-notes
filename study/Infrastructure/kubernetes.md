# What is Kubernetes

Open source container orchertration tool, developed by Google. Helps manage containerized applications in different deployments enviroments (Physical, virtual or cloud).

==> What features do orchestration tools offer ?

> High availability or no downtime
> Scalability or high performance
> Disaster recovery: backup and restore

## Main Kubernetes Components summarized

Taking a botton-up approach.

### Container

Stand-alone packages that include everything needed to run a piece of software, including code, runtime, libraries, and system tools.

### Pod:

Smallest deployable units in Kubernetes. They represent one or more containers that share the same network namespace and storage. Usually schedule together on the same node.

### Node: 

worker machines in a kubernetes cluster. Run pods and other kubernetes components.

### Service: 

network abstraction to access a set of Pods as a single stable endpoint. Allow apps to comunicate with each other or external clients.


### Ingress:

Kubernetes resource that manages external access to services within the cluster, typically for https routing. Allow define routing rules, such as path-based or host-based routing.

### ConfigMap: 

ConfigMaps are used to store configuration data in key-value pairs. They help decouple configuration from pods and make it easier to manage and update config settings.

### Secret

Similar to ConfigMaps but are used to store sensitive data like passwords, API keys, or certificates. They are base64-encoded and can be mounted into Pods as files or used as environment variables.

### Volumes

used to persist data in Kubernetes Pods. They allow containers within the same Pod to share storage or provide a way to store data beyond the lifecycle of a Pod.

### Stateless Applications

designed in a way that they do not rely on persistent local data or state. Each request to a stateless application is independent and can be processed without any knowledge of previous requests.

### Stateful applications

maintain some form of persistent state or data that is crucial for the app operation. Rely on data that persists across multiple requests or instances of the application.


## Node Processes

Kubernetes (K8s) has a hierarchical architecture that consists of different components, each with its specific role in managing containerized applications.

### Worker Nodes

are the machines in a Kubernetes cluster where containers run. They are responsible for executing the containers and providing the runtime environment.

- Each node has multiple Pods on it. Each worker node hosts multiple instances of these Pods, and each Pod may be running one or more containers. These Pods are isolated from each other but share the same underlying node resources, such as CPU and memory.

- Kubernetes requires specific processes to be installed and running on each worker node to manage containers and Pods effectively. These processes ensure that containers are scheduled, managed, and networked correctly:


1. **Container Runtime**

- Role: The Container Runtime is responsible for executing containers. It is the software that runs and manages containers, such as Docker..
- Function: It ensures that containers are started, stopped, and managed efficiently on the node.
- Must be installed on every node.

1. **Kubelet** 

- Role: The Kubelet is a critical component that runs on each node and acts as the node-level agent of the Kubernetes control plane.

- Function: It interacts with both the container runtime and the control plane. is responsible for starting and stopping Pods with their containers and ensuring that they maintain the desired state as defined in the cluster's configuration.

- Interacts with both container and node. And it is responsible to start the pod with a container inside.

2. **Kube proxy** 

- Role: Kube Proxy is another essential component that runs on each node.

- Function: It maintains network rules on the node, enabling network communication to and from the Pods running on that node. Kube Proxy helps in providing the networking layer for inter-Pod and external communication.

**In summary, for each worker node in a Kubernetes cluster, you have the Container Runtime responsible for running containers, the Kubelet responsible for managing Pods and containers, and the Kube Proxy responsible for networking tasks. These processes are essential for the proper functioning of a Kubernetes cluster and the execution of containerized applications.**


### Master Nodes

- Role: Master Nodes are responsible for managing the overall cluster and making global decisions about the cluster (such as scheduling). They don't run the application containers themselves but coordinate and control the cluster.

### Api Server


- Role: The API Server is the entry point to the Kubernetes control plane. It exposes the Kubernetes API, which is used by all other components and users to interact with the cluster.

### Scheduler

- Role: The Scheduler is responsible for deciding which Worker Node should run a new Pod based on res ource requirements, affinity/anti-affinity rules, and other policies.

- Evaluate the resources available, looking to the one with more bandwidth. 

### Controller manager

Role: The Controller Manager consists of several controller processes that regulate the state of the system, ensuring that the desired state (as defined in configurations) is maintained. Basically detects state changes like a pod dying.

### etcd

Role: etcd is a distributed key-value store that serves as the source of truth for the cluster. It stores all configuration data and the state of the cluster.

### Conclusion

**The Worker Nodes do the actual work, the Master Nodes provide management and coordination, the API Server is the interface, the Scheduler makes decisions, the Controller Manager enforces policies, and etcd is the source of truth.**



## Kubectl main commands

> kubectl get nodes


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

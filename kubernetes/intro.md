# Kubernetes
---

## Kubernetes Components
---

1. **Node**
  - Single machine virtual or physical
2. **Pod**
  - Smallest unit of K8s
  - Abstraction over container
  - Usually 1 application per pod
  - Each pod get its own IP address
  - Assigned new IP address on re-creation
3. **Service**
  - Permanent IP address
  - Abstract a set of Pods
  - [Read More about different types of services...](#service-types)
4. **Ingress**
  - Manage accepting external request and forward it to service
5. **ConfigMap**
  - External configuration for the application
6. **Secrets**
  - Used to store secret data e.g (password, certs)
  - Base64 encoded
7. **Volumes**
  - Storage can be on local machine or remote outside of the k8s cluster
  - A directory which is accessible to Pods Job

  > [!note] 
  > K8s doesnt manage data persistence

8. **Deployment**
  - Blueprint for apps pods
  - Abstraction of pods
  - Ideal for stateless apps
  
  > [!note] 
  > DB can't be replicated via deployment

9.  **Statefulset**
  - Ideal for stateful apps or databases
  - Same like deployment but for stateful apps


## Kubernetes Architecture
---

### master node
4 processes run on every master node
	*api server*
	*scheduler*
	- just decides on which node new Pod should be scheduled
	*controller manager*
	- detects cluster state changes
	*etcd*
	- is the cluster brain
	- cluster changes get stored in the key value store

### node
- worker machine in k8s cluster
- can be vm, azure aci, fargate

### kubelet
- interacts with container and node
- starts the pod with the container inside

### kube proxy
- forwards the requests

> [!abstract] Layers of Abstraction
> - *Deployment* manages a
> - *ReplicaSet* manages a
> - *Pod* is an abstraction of a
> - *Container*

## Namespaces
---

### What is Namespace
- Organise resources in namespaces
- Virtual cluster inside a cluster
- 4 default namespace
  - default
  - kube-node-lease
  - kube-public
  - kube-system

### When to use namespaces
- Structure your components
- Avoid conflicts within teams
- Share service between difference environments
- Access and resource limits on Namespace level
  
### Characteristics of Namespace
- You cant access most resources from another Namespaces
- Access service in another namespace
- Components, which can't be created with a Namespace
  - Live globally in a cluster
  - you can't isolate them

### Best practice
- Create namespace inside configuration file / manifest for better documentation


## Service Types
---

### ClusterIP

The primary purpose of ClusterIP service is exposing groups of pods to other pods in the cluster. Choosing this value makes the service reachable only from within the cluster. This is the default ServiceType

### Node Port

You’ll also want to expose services such as frontend webservers to the outside, so that external clients can access them as depicted below.

The first method of exposing a set of pods to external clients is to create a service and set its type field to NodePort.

If you set the type field to NodePort, the Kubernetes control plane allocates a port from a range (default: 30000-32767). Each node proxies that port (the same port number on every Node) into your service.

The diagram below shows an external client connecting to a NodePort service, either through node 1 or 2.

Your service reports the allocated port in its .spec.ports[*].nodePort field.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: NodePort
  selector:
    app.kubernetes.io/name: MyApp
  ports:
    – port: 8080
      targetPort: 8080
      nodePort: 30123
```

### Load Balancer

Kubernetes clusters running on public clouds usually support the automatic provisioning of a load balancer from the cloud infrastructure. All you need to do is set the service’s type to LoadBalancer instead of NodePort.

The load balancer will have its own unique, publicly accessible IP address and will redirect all connections to your service. In this way, you can access your service through the load balancer’s IP address.

### ExternalName

Instead of exposing an external service by manually configuring the service’s endpoints, a simpler method allows you to refer to an external service by its fully qualified domain name (FQDN).

ExternalName maps the service to the contents of the externalName field, by returning a CNAME record with its value. No proxying of any kind is set up.

This service definition, for example, maps the my-service service in the pod namespace to *my.database.example.com*.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-database-svc
  namespace: prod
spec:
  type: ExternalName
  externalName: my.database.example.com
```

## Ingress
---

## External DNS
---
- This tool will create necessary DNS records in your externaal DNS servers (like route53, digitalocean, linode etc..)
- For every hostname that you use in ingress, it'll create a new record to send traffic to your loadbalancer
- Major DNS provider are supported: Google CloudDNS, Route53, AzureDNS, CloudFlare, DigitalOcean, Linode etc...




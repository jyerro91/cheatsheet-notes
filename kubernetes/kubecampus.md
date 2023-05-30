## Kubecampus
---

## Kubernetes Concepts and Building a K8s Cluster
---

To work with **Kubernetes**, you use Kubernetes API objects to describe your cluster’s **desired state**: what applications or other workloads you want to run, what container images they use, the number of replicas, what network and disk resources you want to make available, and more.

**Kubernetes Control Plane** is a collection of three processes that run on a single node in your cluster, which is designated as the Control Plane node. Those processes are: *kube-apiserver*, *kube-controller-manager* and *kube-scheduler*.

Each individual non-Control Plane node in your cluster runs two processes:

- *kubelet* which communicates with the Kubernetes Control Plane.
- *kube-proxy* a network proxy which reflects Kubernetes networking services on each node.

The various parts of the Kubernetes Control Plane, such as the kube-apiserver and kubelet processes, govern how Kubernetes communicates with your cluster. The Control Plane maintains a record of all of the Kubernetes Objects in the system, and runs continuous control loops to manage those objects’ state. At any given time, the Control Plane’s control loops will respond to changes in the cluster and work to make the actual state of all the objects in the system match the desired state that you provided.

![[control-plane.png]]


## Kubernetes Control Plane

The Kubernetes Control Plane is responsible for maintaining the desired state for your cluster. When you interact with Kubernetes, such as by using the kubectl command-line interface, you’re communicating with your cluster’s Kubernetes Control Plane.

The “Control Plane” refers to a collection of processes managing the cluster state. Typically these processes are all run on a single node in the cluster, and this node is also referred to as the Control Plane. The Control Plane can also be replicated for availability and redundancy.

## Kubernetes Nodes

The nodes in a cluster are the machines (VMs, physical servers, etc) that run your applications and cloud workflows. The Kubernetes Control Plane controls each node; you’ll rarely interact with nodes directly.

![[nodes.png]]


Kubernetes contains a number of abstractions that represent the state of your system: deployed containerized applications and workloads, their associated network and disk resources, and other information about what your cluster is doing. These abstractions are represented by objects in the [Kubernetes API](https://kubernetes.io/docs/concepts/overview/kubernetes-api/).

A Pod is the basic building block of Kubernetes–the smallest and simplest unit in the Kubernetes object model that you create or deploy. A Pod represents a running process on your cluster.

A Pod encapsulates an application container (or, in some cases, multiple containers), storage resources, a unique network IP, and options that govern how the container(s) should run. A Pod represents a unit of deployment: a single instance of an application in Kubernetes, which might consist of either a single container or a small number of containers that are tightly coupled and that share resources.

Pods in a Kubernetes cluster can be used in two main ways:

- **Pods that run a single container**. The “one-container-per-Pod” model is the most common Kubernetes use case; in this case, you can think of a Pod as a wrapper around a single container, and Kubernetes manages the Pods rather than the containers directly.
    
- **Pods that run multiple containers that need to work together**. A Pod might encapsulate an application composed of multiple co-located containers that are tightly coupled and need to share resources. These co-located containers might form a single cohesive unit where one container serving files from a shared volume to the public, while a separate container refreshes or updates those files. The Pod wraps these containers and storage resources together as a single manageable entity.

Each Pod is meant to run a single instance of a given application. If you want to scale your application horizontally (e.g., run multiple instances), you should use multiple Pods, one for each instance.

In Kubernetes, this is generally referred to as replication. Replicated Pods are usually created and managed as a group by an abstraction called a Controller, for example a **ReplicaSet** controller (to be discussed) maintains the **Pod** lifecycle. This includes **Pod** creation, upgrade and deletion, and scaling.

See [Pods](https://kubernetes.io/docs/concepts/workloads/pods/) and [Controllers](https://kubernetes.io/docs/concepts/architecture/controller/) for more information.

Each Pod is meant to run a single instance of a given application. If you want to scale your application horizontally (e.g., run multiple instances), you should use multiple Pods, one for each instance.

In Kubernetes, this is generally referred to as replication. Replicated Pods are usually created and managed as a group by an abstraction called a Controller, for example a **ReplicaSet** controller (to be discussed) maintains the **Pod** lifecycle. This includes **Pod** creation, upgrade and deletion, and scaling.

See [Pods](https://kubernetes.io/docs/concepts/workloads/pods/) and [Controllers](https://kubernetes.io/docs/concepts/architecture/controller/) for more information.

![[pods.svg]]

A **ReplicaSet** controller ensures that a specified number of **Pod** replicas are running at any given time.

While **ReplicaSets** can be used independently, today it’s mainly used by **Deployments** as a mechanism to orchestrate **Pod** creation, deletion and updates. When you use **Deployments** you don’t have to worry about managing the **ReplicaSets** that they create. **Deployments** own and manage their **ReplicaSets**.

---

A **Deployment** controller provides declarative updates for **Pods** and **ReplicaSets**.

You describe a desired state in a **Deployment** object, and the **Deployment** controller changes the actual state to the desired state at a controlled rate. You can define **Deployments** to create new **ReplicaSets**, or to remove existing **Deployments** and adopt all their resources with new **Deployments**

**Deployment** manages the **ReplicaSet** to orchestrate **Pod** lifecycles. This includes **Pod** creation, upgrade and deletion, and scaling.

--- 

A Kubernetes **Service** is an abstraction which defines a logical set of **Pods** and a policy by which to access them - sometimes called a micro-service. The set of **Pods** targeted by a **Service** is (usually) determined by a **Label Selector**.

As an example, consider an image-processing backend which is running with 3 replicas. Those replicas are fungible - frontends do not care which backend they use. While the actual **Pods** that compose the backend set may change, the frontend clients should not need to be aware of that or keep track of the list of backends themselves. The **Service** abstraction enables this decoupling.

For Kubernetes-native applications, Kubernetes offers a simple Endpoints API that is updated whenever the set of **Pods** in a **Service** changes. For non-native applications, Kubernetes offers a virtual-IP-based bridge to Services which redirects to the backend **Pods**.

---

**Namespaces** are intended for use in environments with many users spread across multiple teams, or projects.

Namespaces provide a scope for names. Names of resources need to be unique within a namespace, but not across namespaces.

Namespaces are a way to divide cluster resources between multiple users (via resource quota).

>[!tip]
>It is not necessary to use multiple namespaces just to separate slightly different resources, such as different versions of the same software: use labels to distinguish resources within the same namespace.


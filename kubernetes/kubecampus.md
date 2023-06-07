## Kubecampus
---

- **kubelet** : The primary agent that runs on a kubernetes node and communicates with the control plane to issue commands to this node and report status
- **kubeadm** : A tool that performs the action necessary to build up a Kubenertes cluster
- **kubectl** : The command line tool that lets you control kubernetes clusters and issue commands to the api server
- **Docker Engine** : A container engine used by Kubernetes to run containers


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

The *nodes* in a cluster are the *machines (VMs, physical servers, etc)* that run your applications and cloud workflows. The Kubernetes Control Plane controls each node; you’ll rarely interact with nodes directly.

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

---

On-disk files in a Container are ephemeral, which presents some problems for non-trivial applications when running in Containers. First, when a Container crashes, **kubelet** will restart it, but the files will be lost - the Container starts with a clean state. Second, when running Containers together in a **Pod** it is often necessary to share files between those Containers. The Kubernetes **Volume** abstraction solves both of these problems.

At its core, a **Volume** is just a directory, possibly with some data in it, which is accessible to the Containers in a **Pod**. How that directory comes to be, the medium that backs it, and the contents of it are determined by the particular volume type used.

A Kubernetes **Volume** has an explicit lifetime - the same as the **Pod** that encloses it. Consequently, a volume outlives any Containers that run within the **Pod**, and data is preserved across Container restarts. When a **Pod** ceases to exist, the volume will cease to exist too. Kubernetes supports many types of **Volumes**, and a Pod can use any number of them simultaneously.

Some examples of **Volumes** are:

- emptyDir - just an empty directory
- azureDisk - a disk on Microsoft Azure
- hostPath - a directory that lives on the node itself
- nfs - an exported NFS share

but there are many more. Take a look [here](https://kubernetes.io/docs/concepts/storage/volumes/#types-of-volumes).

---

A **Job** creates one or more **Pods** and ensures that a specified number of them successfully terminate.

As **Pods** successfully complete, the **Job** tracks the successful completions. When a specified number of successful completions is reached, the **Job** itself is complete. Deleting a **Job** will clean up the **Pods** it created.

A simple case is to create one **Job** object in order to reliably run one **Pod** to completion. The **Job** object will start a new **Pod** if the first **Pod** fails or is deleted (for example due to a node hardware failure or a node reboot).

A **Job** can also be used to run multiple **Pods** in parallel.

---

A **DaemonSet** ensures that all (or some) Nodes run a copy of a **Pod**.

As nodes are added to the cluster, **Pods** are added to them. As nodes are removed from the cluster, those **Pods** are garbage collected. Deleting a **DaemonSet** will clean up the **Pods** it created.

Some typical uses of a **DaemonSet** are:

- running a cluster storage daemon, such as `glusterd` and `ceph` on each node.
- running a logs collection daemon on every node, such as `fluentd` or `logstash`.
- running a node monitoring daemon on every node, such as `Prometheus Node Exporter`, `collectd`, `Datadog agent`, `New Relic agent`, or `Ganglia gmond`.

---

A **StatefulSet** manages the **deployment** and scaling of a set of **Pods**, and provides guarantees about the ordering and uniqueness of these **Pods**.

**StatefulSets** are valuable for applications that require one or more of the following.

- Stable, unique network identifiers.
- Stable, persistent storage.
- Ordered, graceful deployment and scaling.
- Ordered, graceful deletion and termination.
- Ordered, automated rolling updates.

Use cases for **StatefulSets** are:

- Deploying a clustered resource (e.g. Cassandra, Elasticsearch)
- Applications that somehow depend on each other


## Kubernetes Application Lab
---

**Containers** allow you to package your application and its dependencies together into one succinct manifest that can be version controlled, allowing for easy replication of your application across developers on your team and machines in your cluster.

**Containers** work best for service based architectures. As opposed to monolithic architectures, where every piece of the application is intertwined — from IO to data processing to rendering — service based architectures separate these into separate components.

From an application point of view, instantiating an image (creating a container) is similar to instantiating a process like a service or a web app.

---

A **stateless** app is an application program that does not save client data generated in one session for use in the next session with that client e.g. print services, microservices.

In **stateful** applications, the state is recorded. By state, we mean any changeable occurrence that includes internal operations, interactions with other applications, environment variables, user-set preferences, memory content, and temporary storage. The data that such applications store depends on their types and other factors under which they operate. Usually, a stateful application is able to record your preferences, track your window size and location, and memorize the files that you have recently opened. Some known examples of stateful applications include MongoDB, Cassandra, and MySQL.

---

A **Container Image**, in its simplest definition, is a file which is pulled down from a Registry Server and used locally as a mount point when starting Containers. A container is the runtime instantiation of an image.

A **Container Engine** is a piece of software that accepts user requests, including command line options, pulls images, and from the end user's perspective runs the container.

There are many container engines, including Docker, RKT, and CRI-O that provide the runtime environment for the applications inside the container.

You can run them locally or on a remote server.

**Docker** containers that run on Docker Engine are lightweight, portable and secure. They run everywhere: Linux, Windows, Data center, Cloud, Serverless, etc.

[Docker Build](https://docs.docker.com/develop/) is at the core of what makes Docker so popular. It allows you to easily create and share portable Docker container images using open standards and create images for [multiple CPU and OS architectures](https://github.com/docker-library/official-images#architectures-other-than-amd64) and share them in your [private registry](https://www.docker.com/products/image-registry) or on [Docker Hub](https://hub.docker.com/).

[Docker Desktop](https://www.docker.com/products/docker-desktop) is an application for MacOS and Windows machines for the building and sharing of containerized applications and microservices.

[Docker Compose](https://docs.docker.com/compose/) is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration.

---

The Kubernetes storage architecture is based on [Volumes](https://kubernetes.io/docs/concepts/storage/volumes/) as a central abstraction.

**Volumes** can be persistent or non-persistent, which allows containers to request storage resources dynamically, using a mechanism called volume claims.

#### StorageClasses

Kubernetes users can define StorageClasses and assign [PVs](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) to them. Each [StorageClass](https://kubernetes.io/docs/concepts/storage/storage-classes/) represents a type of storage and uses provisioners that determines what volume plugin is used for provisioning PVs.

A [StorageClass](https://kubernetes.io/docs/concepts/storage/storage-classes/) provides a way for administrators to describe the "classes" of storage they offer. Different classes might map to quality-of-service levels, or to backup policies, or to arbitrary policies determined by the cluster administrators. Kubernetes itself is unopinionated about what classes represent. This concept is sometimes called "profiles" in other storage systems

Check the current storage classes created in the cluster
```shell
kubectl get storageclasses
```


#### Persistent Volumes (PV)

A PersistentVolume (PV) is a piece of storage in the cluster that has been provisioned by an administrator or dynamically provisioned using Storage Classes. It is a resource in the cluster just like a node is a cluster resource. PVs are volume plugins like Volumes, but have a lifecycle independent of any individual Pod that uses the PV. This API object captures the details of the implementation of the storage, be that NFS, iSCSI, or a cloud-provider-specific storage system.

A [PersistentVolume](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistent-volumes) can be mounted on a host in any way supported by the resource provider.

Providers will have different capabilities and each PV's access modes are set to the specific modes supported by that particular volume.

For example, NFS can support multiple read/write clients, but a specific NFS PV might be exported on the server as read-only.

Each PV gets its own set of access modes describing that specific PV's capabilities.

The access modes are:

- ReadWriteOnce -- the volume can be mounted as read-write by a single node
- ReadOnlyMany -- the volume can be mounted read-only by many nodes
- ReadWriteMany -- the volume can be mounted as read-write by many nodes

We are going to create a PersistentVolume of 10Gi, called 'myvolume'.

Make it have accessMode of 'ReadWriteOnce' and 'ReadWriteMany', storageClassName 'local-path', mounted on hostPath '/etc/foo'.

A hostPath PersistentVolume uses a file or directory on the Node to emulate network-attached storage.

Let's create the directory that we are going to use in this example:

```yaml
kind: PersistentVolume
apiVersion: v1
metadata:
  name: myvolume
spec:
  storageClassName: local-path
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
    - ReadWriteMany
  hostPath:
    path: /etc/foo
```

#### Persistent Volume Claims

A PersistentVolumeClaim (PVC) is a request for storage by a user. It is similar to a Pod. Pods consume node resources and PVCs consume PV resources. Pods can request specific levels of resources (CPU and Memory). Claims can request specific size and access modes (e.g., they can be mounted ReadWriteOnce, ReadOnlyMany or ReadWriteMany)

Pods cannot access Persistent Volumes directly, we need to claim storage capacity for our applications by binding the request for capacity to PVs using [PersistentVolumeClaims](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistentvolumeclaims).

We are going to create a PersistentVolumeClaim, called 'mypvc', with a request of 4Gi and an accessMode of ReadWriteOnce.

```yaml
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: mypvc
spec:
  storageClassName: local-path
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 4Gi
```

---


### Introduction to Kubestr

## Kubestr

Kubestr is a collection of tools to discover, validate and evaluate your kubernetes storage options.

As adoption of Kubernetes grows so have the persistent storage offerings that are available to users. The introduction of [CSI](https://kubernetes.io/blog/2019/01/15/container-storage-interface-ga/) (Container Storage Interface) has enabled storage providers to develop drivers with ease. In fact there are around a 100 different CSI drivers available today. Along with the existing in-tree providers, these options can make choosing the right storage difficult.

Kubestr can assist in the following ways:

- Identify the various storage options present in a cluster.
- Validate if the storage options are configured correctly.
- Evaluate the storage using common benchmarking tools like FIO.

## Using Kubestr

To install the tool

- Download the latest release:
    
    `curl -sLO https://github.com/kastenhq/kubestr/releases/download/v0.4.17/kubestr-v0.4.17-linux-amd64.tar.gz`
    
- Unpack the tool and make it an executable chmod +x kubestr.
    
    `tar -xvzf kubestr-v0.4.17-linux-amd64.tar.gz -C /usr/local/bin chmod +x /usr/local/bin/kubestr`
    

To discover available storage options: Run `kubestr`

To run an FIO test

Using `kubestr` we can test sequential read and write speeds which are expected to be high.

Random reads and writes can be tested as well. Moreover, random writes separates good drives from the bad ones.

- Copy the test specification:
    
    `cat <<'EOF'>ssd-test.fio [global] bs=4k ioengine=libaio iodepth=1 size=1g direct=1 runtime=10 directory=/ filename=ssd.test.file [seq-read] rw=read stonewall  [rand-read] rw=randread stonewall  [seq-write] rw=write stonewall  [rand-write] rw=randwrite stonewall EOF`
    
- Run the following command to start the test
    
    `kubestr fio -f ssd-test.fio -s local-path`
    
- Additional options like `--size` and `--fiofile` can be specified.
    
- For more test examples visit our [FIO](https://github.com/axboe/fio/tree/master/examples) page.
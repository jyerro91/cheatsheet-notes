---
title: Kubernetes Volumes
date: 2023-06-13
tags: [kubernetes, orchestration, automation, volumes]
---

# Kubernetes Volumes
---

Kubernetes storage is based on the concepts of volumes: there are ephemeral volumes that are often simply called “volumes” and there are “Persistent Volumes” that are meant for long-term storage.

A volume can be thought of as a directory that is accessible to the container in a pod. A pod can use any number of volume types simultaneously.

Kubernetes supports several types of volumes:

- emptyDir
- hostPath
- Local
- nfs
- fc (fibre channel)
- gcePersistentDisk
- awsElasticBlockStore
- azureDisk
- azureFile
- Cephfs

**CSI (Container Storage Interface)** is a standard for exposing arbitrary block filestore systems to the containerized workload on container orchestration systems such as Kubernetes. CSI providers can write plugins, and those can be used in Kubernetes to extend storage capabilities.


#### Kubernetes Volumes and Storage Types

In Kubernetes, all containers are ephemeral and a Kubernetes volume is an abstraction implemented to solve two problems:

1. Loss of files when a container crashes
2. Sharing files between containers running together in a pod

Volumes fall under three major categories:

- Persistent volumes
- Ephemeral volumes
- Projected volumes

Persistent volumes exist beyond the lifetime of a pod.  Ephemeral volumes will be destroyed when a pod is destroyed. A projected volume maps several existing volume sources into the same directory.

**Storage Classes** 

Storage classes provide a way to describe different types of storage available for Kubernetes. Storage classes are defined by [Kubernetes cluster](https://kubecampus.io/kubernetes/courses/first-kubernetes-cluster/) administrators using the StorageClass Resource.

Administrators can map different classes based on quality-of-service levels and backup policies, or to arbitrary policies derived from storage requirements.

Each storage class has an associated provisioner that determines what volume plugin is used for provisioning the persistent volumes.

**Example plugins:**

- AWS EBS
- Azure File
- NFS
- Cinder
- OpenStack Cinder

#### Ephemeral Volumes

Generic ephemeral volumes are a newer feature and are in beta (enabled by default) as of Kubernetes 1.21. These volumes also work with typical storage operations such as snapshotting, cloning, resizing, and storage capacity tracking.

**Types of ephemeral storage:**

- emptyDir
- configMap, downwardAPI, secret
- CSI ephemeral volumes
- generic ephemeral volumes

More information on this topic is available [here](https://kubernetes.io/docs/concepts/storage/ephemeral-volumes/).

#### Persistent Volumes

A persistent volume is a resource in the cluster and has a life cycle independent of any individual pod that uses persistent volumes. Examples of persistent volumes are:

- NFS
- iSCSI

A volume claim is a request for storage by a user. It is similar to a pod. Pods consume node resources, and PVCs consume PV resources.

**Persistent Volume Lifecycle**

The persistent volume lifecycle consists of these components:

- Provisioning
- Static
- Dynamic
- Binding
- Using
- Reclaiming
- Retaining
- Deleting

More information on this topic can be found [here](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#lifecycle-of-a-volume-and-claim).

#### Projected Volumes

A projected volume maps several existing volume sources into the same directory.

It automatically populates a single volume to create a single directory with the keys from multiple Secrets, ConfigMaps and downstream API information.

More information on this topic can be found at this [link](https://kubernetes.io/docs/concepts/storage/projected-volumes)

#### Kubernetes Volume Snapshots and Volume Snapshot Classes

Volume snapshots provide a standardized way to copy volumes of content at a particular point in time, without creating a new volume. This capability enables administrators to perform various backup operations in Kubernetes.

A VolumeSnapshotContent is a snapshot taken from a volume in the cluster that has been provisioned by an administrator. It is a resource in the cluster just like a PersistentVolume is a cluster resource.

A VolumeSnapshot is a request for a snapshot of a volume by a user. It is similar to a PersistentVolumeClaim.

VolumeSnapshotClass enables you to specify different attributes belonging to a VolumeSnapshot. These attributes may differ among snapshots taken from the same volume on the storage system and, therefore, cannot be expressed by using the same StorageClass of a PersistentVolumeClaim.

More information on this topic can be found [here.](https://kubernetes.io/docs/concepts/storage/volume-snapshots/)


## Related Resources
---

[kubernetes-storage-explained](https://medium.com/swlh/kubernetes-storage-explained-558e85596d0c)

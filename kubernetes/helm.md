# Helm
---

[Helm](https://helm.sh/) is an open source tool used for packaging and deploying applications on Kubernetes.

It is often referred to as the Kubernetes Package Manager because of its similarities to any other package manager you would find on your favorite OS.

Helm is widely used throughout the Kubernetes community and is a CNCF graduated project.

Given Helm's similarities to traditional package managers, let's begin exploring Helm by first reviewing how a package manager works.

**Helm** uses [Charts](https://helm.sh/docs/topics/charts/) to pack all the required K8S components for an application to deploy, run and scale. A chart is a collection of files that describe a related set of Kubernetes resources. A single chart might be used to deploy something simple, like a memcached pod, or something complex, like a full web app stack with HTTP servers, databases, caches, and so on.

**Helm** [Templates](https://helm.sh/docs/chart_best_practices/templates/) is subdirectory in a chart that combines the K8S components of it, e.g. Service, ReplicaSet, Deployment, Ingress etc.

**Helm** [Values](https://helm.sh/docs/chart_best_practices/values/) are described in the values.yaml file which allows users to deploy their containerized applications dynamically. Its Yaml structure holds values that matches the templates defined in the application manifest.

## Understanding Package Managers
---

Package managers are used to simplify the process of installing, upgrading, reverting, and removing a system's applications. These applications are defined in units, called packages, which contain metadata around target software and its dependencies.

The process behind package managers is simple:

- First, the user passes the name of a software package as an argument.
- The package manager then performs a lookup against a package repository to see whether that package exists. If it is found, the package manager installs the application defined by the package and its dependencies to the specified locations on the system.


## Installing the Helm Client
---

Helm provides a single command-line client that is capable of performing all of the main Helm tasks. This client is, appropriately enough, named `helm`.

The `helm` client is written in a programming language called Go.

In what follows in the lab, we are going to take a look at the most common workflow for starting out with Helm:

- Install the Helm client.
- Add a chart repository.
- Find a chart to install.
- Install a Helm chart.
- See the list of what is installed.
- Upgrade your installation.
- Delete the installation.

## Adding a Chart Repository
---

A Helm chart is an individual package that can be installed into your Kubernetes cluster. During chart development, you will often just work with a chart that is stored on your local filesystem.

But when it comes to sharing charts, Helm describes a standard format for indexing and sharing information about Helm charts.

A Helm **chart repository** is simply a set of files, reachable over the network, that conforms to the Helm specification for indexing packages.

A chart repository is an HTTP server that houses an **index.yaml** file and optionally some packaged charts. When you're ready to share your charts, the preferred way to do so is by uploading them to a chart repository.

Adding a Helm chart is done with the `helm repo add` command.

### Searching a Chart Repository

The easiest way to find the popular repositories is to use your web browser to navigate to the [Artifact Hub](https://artifacthub.io/). There you will find thousands of Helm charts, each hosted on an appropriate repository.

You can also use the command `helm search repo REPOSITORY_NAME` to search for a chart in an added repository.

### Installing an Application

At a minimum, installing a chart in Helm requires two pieces of information:

1. **The name of the installation**
2. **The chart you want to install**

For example, use the command `helm install RELEASE_NAME CHART` to install a single instance of the chart.


### Configuration at Installation Time

While the default configuration is good sometimes, more often we want to pass our own configuration to the chart.

Many charts will allow you to provide configuration values. If we take a look at the Artifact Hub page for Wordpress, we would see a long list of configurable parameters. For example, we can configure the `username` of the Wordpress admin account by setting the `wordpressUsername` value.

You can use the `--set` flag which takes one or more values directly to add individual parameters to an install or upgrade.

For example `helm install mysite bitnami/drupal --set wordpressUsername=admin`

You can use the `--values` flag which takes a file directly to add individual parameters to an install or upgrade.

For example `helm install mysite bitnami/drupal --values prd-values.yaml`


### Install Kubernetes Applications

## Deploy Wordpress

We're going to demonstrate deploying a Kubernetes application using the [helm install](https://helm.sh/docs/helm/helm_install/) command.

Create a namespace for the installation target:

```shell
kubectl create namespace wordpress
```

Use the install command to deploy the chart to your cluster:

```shell
helm install my-wordpress bitnami/wordpress --set wordpressUsername=admin --set service.type=NodePort --set wordpressPassword=password --set mariadb.auth.rootPassword=secretpassword --namespace wordpress
```

With the install command, Helm will launch the required Deployments, ReplicaSets, Pods, Services, ConfigMaps, or any other Kubernetes resource the chart defines.

The notes will provide helpful information on how to access the new services.

You can inspect the deployment of the application resources using the following command:

```shell
kubectl get all -n wordpress
```

Use the [helm list](https://helm.sh/docs/helm/helm_list/) command with the `--all-namespaces` option to list releases across all namespaces.

```shell
helm list --all-namespaces
```

Or list releases to a specific namespace scope:

```shell
helm ls -n wordpress
```

## Chart Installation Information

Deployment information is maintained in a secret stored on the targeted Kubernetes cluster.

```shell
kubectl get secrets --all-namespaces --selector owner=helm
```

For the Wordpress chart, you installed to the wordpress namespace you can see the secret information about the deployment:

```shell
kubectl --namespace wordpress describe secret sh.helm.release.v1.my-wordpress.v1
```

Notice the field Release which stores the release state.

```shell
kubectl --namespace wordpress get secret sh.helm.release.v1.my-wordpress.v1 -o jsonpath='{.data.release}' | base64 -d | base64 -d | gunzip -c
```

However, the Helm client offers a friendly way to inspect your release directly using the [helm get](https://helm.sh/docs/helm/helm_get/) commands:

```shell
helm get all - get all information for a named release helm get hooks - get all hooks for a named release helm get manifest - get the manifest for a named release helm get notes - get the notes for a named release helm get values - get the values file for a named release
```

Run the following commands to see the output:

```shell
helm get manifest my-wordpress --namespace wordpress
helm get notes my-wordpress --namespace wordpress
helm get values my-wordpress --namespace wordpress
helm get all my-wordpress --namespace wordpress
```

The difference between `get` and `show` is that the former helps you get information about a release while the latter helps you get information about a chart.

A release is simply a deployment instance of a chart.

When ready, click on the `Check` button to continue.
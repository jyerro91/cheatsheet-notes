# Custom Resource Definitions
---
### Understanding CRDs

- A **resource** is an endpoint in the [Kubernetes API](https://kubernetes.io/docs/concepts/overview/kubernetes-api/) that stores a collection of [API objects](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/) of a certain kind; for example, the built-in Pods resource contains a collection of Pod objects. 
- A **custom resource** is an extension of the Kubernetes API that is not necessarily available in a default Kubernetes installation. It represents a customization of a particular Kubernetes installation. However, many core Kubernetes functions are now built using custom resources, making Kubernetes more modular. 
- Custom resources can appear and disappear in a running cluster through dynamic registration, and cluster admins can update custom resources independently of the cluster itself. Once a custom resource is installed, users can create and access its objects using kubectl or other Kubernetes API clients, just as they do built-in resources like **Pods**. 

### Understanding Custom Resource APIs

- Custom resources let you store and retrieve structured data. When you combine a custom resource with a custom controller, custom resources provide a true API model. 
- Custom resource APIs can be either declarative or imperative. The Kubernetes [declarative API](https://kubernetes.io/docs/concepts/overview/kubernetes-api/) enforces a separation of responsibilities. You declare the desired state of your resource. The Kubernetes controller keeps the current state of Kubernetes objects in sync with your declared desired state. This is in contrast to an imperative API, where you instruct a server what to do. 
- You can deploy and update a custom controller on a running cluster, independently of the cluster’s lifecycle. Custom controllers can work with any kind of resource, but they are especially effective when combined with custom resources. The [Operator pattern](https://kubernetes.io/docs/concepts/extend-kubernetes/operator/) combines custom resources and custom controllers. You can use custom controllers to encode domain knowledge for specific applications into an extension of Kubernetes.  
    

- **In a Declarative API, typically:** 
    - Your API consists of a relatively small number of relatively small objects (resources). 
    - The objects define configuration of applications or infrastructure. 
    - The objects are updated relatively infrequently. 
    - Humans often need to read and write the objects. 
    - The main operations on the objects are CRUD-y (creating, reading, updating and deleting). 
    - Transactions across objects are not required: the API represents a desired state, not an exact state.  
         
- **Imperative APIs** are not declarative. Signs that your API might not be declarative include: 
    - The client says, “do this.” then gets a synchronous response back when it is done. 
    - The client says, “do this,” then gets an operation ID back, and has to check a separate Operation object to determine completion of the request. 
    - You talk about Remote Procedure Calls (RPCs). 
    - Directly storing large amounts of data; for example, > a few kB per object, or > 1000s of objects. 
    - The natural operations on the objects are not CRUD-y. 
    - The API is not easily modeled as an object. 
    - You chose to represent pending operations with an operation ID or an operation object. 

### How does a Custom Resource-based API work?

- In the general Kubernetes model, clients change the custom resource to reflect the desired state. 
- The custom controller detects the difference between the desired state and the actual state of the custom resource and takes actions to bring the actual state in line with the desired state. 
- The status of the custom resource is updated in the Kubernetes API to reflect whether the desired state and actual state are the same and whether there were any errors. 
- The custom controller continuously monitors the current state of the custom resource and takes actions to bring it in line with the desired state, even if there are errors or if the controller is restarted. 

### Common CR-based application patterns:

- **Operator**: A type of application that manages other services via Kubernetes but is not itself the service being managed, such as a database operator that creates and manages a database service from a single custom resource, or a cluster API that creates and manages Kubernetes clusters and nodes. 
- **System Utility**: A type of application that performs a specific function within the Kubernetes system, such as a data protection utility that performs backups and restores of Kubernetes applications using custom resources, or a Kubernetes administration utility that enables interaction with the system only through the utility. 
- **General Purpose Application**: A type of application that can be used for a wide range of purposes. Examples include a CI/CD pipeline that builds, tests, and deploys an application using a single custom resource, or a trading model testing application that runs tests by creating a custom resource for each model and storing the test results in the resource’s status. 

### A proper declarative API design:

Example: 

```yaml
apiVersion: "boxes.example.com/v1"
kind: Box
metadata:
     name: a
spec:
    shelf: 5
status:
   state: Moving/Retrying/Settled
shelf: 5
```
 

### Why is the model shown above a good design?

- The name of the resource (Box) clearly identifies the type of object being represented. 
- The API design uses the “last writer wins” strategy to resolve conflicts, so there is no need to figure out the order in which operations were executed. 
- The design does not require manual cleanup of resources that are no longer needed, so there is no need for garbage collection. 
- The single resource represents the location of the box and handles changes in its location. 
- Reconciliation is simply a matter of checking the differences between the actual state and the declared state of the resource and taking actions to bring the actual state in line with the desired state. 

### Security Considerations:

The standard Kubernetes Role-Based Access Control (RBAC) permissions system can be used to control access to custom resources. 

**Pros:** 

- The existing Kubernetes security model can be used to control access to custom resources. 
- Authentication is handled by the Kubernetes API server, which reduces the burden on the custom resource application to handle authentication. 

**Cons:** 

- Users need to have access to the Kubernetes API server in order to access custom resources. 
- The Kubernetes authentication and authorization system is not designed to allow external users to access the cluster directly. In order to grant access to trusted external users, a gateway must be used, which shifts the responsibility for security back to the application. This means that the application must give Kubernetes credentials to the user, which may not be secure enough for some use cases. 

### Common patterns for controlling access to custom resources:

- Control access to the namespace where the custom resources are stored: 
- Users with Read/Write access to the namespace can use the custom resource application. 
- This is easy to implement but provides only coarse control over access to the custom resources. 
- Create RBAC rules on specific resources: 
- Users can only Read/Write the specific resources that they have permission to access. 
- Users must know the names of the specific resources they want to access and are not able to list all resources. 
- Use a separate namespace for each user: 
- Each user has Read/Write access to the custom resources in their own namespace. 
- The application needs to monitor multiple namespaces. 
- Care must be taken to prevent privilege escalation paths if some shared resources lie in the controller’s namespace. 

### Parallelism

There are several models of parallelism that can be used in CR-based applications. One common approach is to use multiple workers to process work items concurrently. There are several ways to manage the workers, including using a dispatcher/workers model, a queuing system or a publish/subscribe model. 

For CR-based applications, the publish/subscribe model is a common mechanism for implementing parallelism. 

### Advantages and Disadvantages of CR based APIs

**Pros:** 

- Leverages Kubernetes concepts and services: 
- CR-based APIs allow you to use Kubernetes’ built-in role-based access control (RBAC) to manage access to resources. 
- CR-based APIs provide built-in support for create, read, update, and delete (CRUD) operations on resources. 
- You can use the kubectl command-line tool or the Kubernetes API to manage resources through CR-based APIs. 

**Cons:** 

- The application is tied to Kubernetes. This can make it difficult to migrate your application to a different platform or to use it outside of a Kubernetes environment. 
- Declarative API is not always suitable 

### Ways to Implement

There are generally two ways to implement a Kubernetes Resource based application: 

### Using CRDs and Controllers:

- Schema for resources is registered in Kubernetes with CRDs which allow you to define custom resources and their schemas in Kubernetes. 
- A controller is a piece of software that watches for changes to resources and ensures that the desired state is reflected in the actual state of those resources. 

### Using an Aggregated API Server:

- An aggregated API server allows you to add custom endpoints to the Kubernetes API server, which can be used to manage custom resources. 
- The aggregated API server is responsible for handling operations on the custom resources it manages. 

### Key difference:

- Implementing a resource-based application using CRDs and controllers is generally simpler than using an aggregated API server. 
- While an aggregated API server requires more setup and maintenance, it may be more suitable for managing many resources and may offer better performance than using CRDs and controllers.
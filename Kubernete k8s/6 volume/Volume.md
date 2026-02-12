### Kubernetes Volumes: Core Notes

A **Volume** in Kubernetes is essentially a directory that contains data and is accessible to the containers within a Pod. It is a critical component for managing data because, by default, Kubernetes containers start in a clean state after a crash, meaning any data stored inside them is lost.

#### 1. Why Volumes are Necessary
- **Data Persistence:** Prevents data loss when a container crashes or is restarted.
- **Storage Attachment:** It acts as a way to "plug in" physical storage (like a hard drive) to your Pod.
- **Decoupling:** Storage should exist independently of the Pod lifecycle so that data survives even if the Pod is deleted.



#### 2. Persistence Requirements
For storage to be considered truly persistent in a cluster, it must meet three main requirements:
- It must not depend on the specific Pod lifecycle.
- It must be available on all Nodes within the cluster.
- It must survive even if the entire cluster crashes.

#### 3. Key Volume Resources
Kubernetes uses three specific resources to manage persistent storage:
- **Persistent Volume (PV):** A piece of storage in the cluster that has been provisioned by an administrator or dynamically via Storage Classes. PVs are cluster-wide resources and are not limited to a single namespace.
- **Persistent Volume Claim (PVC):** A request for storage by a user. 
- **Storage Class (SC):** Used to provision Persistent Volumes dynamically when a PVC makes a request. It defines the "provisioner" (the storage backend) and specific parameters for the storage.

![](volume_type.png)
#### 4. Local vs. Remote Storage
-  **Remote Storage:** Highly recommended for database persistence. Because it exists outside the Kubernetes cluster, it satisfies the requirements for being available on all nodes and surviving cluster failures.
-  **Local Storage:** Usually tied to a specific Node. This violates persistence requirements because if that specific Node or the cluster crashes, the data may be lost or inaccessible.

#### 5. Volume types
- **emptydir**: empty when pod first created, delete when pod is removed

#### 6. Important Considerations
-  **Management Responsibility:** Kubernetes provides the abstraction for volumes, but it does not manage data persistence itself. You are responsible for tasks like backing up and replicating your data.
-  **Ephemeral Volumes:** Some volume types are ephemeral, meaning they only last as long as the Pod exists and are deleted when the Pod is removed. 
---
## Note
- Do NOT use NodePort Service Type for external connections. Use Ingress or Load Balancer instead.

##### Storage requirements
- Storage that doesn't depend on pod lifecycle 
- Storage must be available on all Nodes 
- Storage needs to survive even if cluster crashes

---
### Link
- K8s volume: https://kubernetes.io/docs/concepts/storage/volumes/
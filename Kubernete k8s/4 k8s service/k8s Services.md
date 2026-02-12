## 1. What is a Kubernetes Service?
A **Service** is a Kubernetes component that provides a static or permanent IP address for a group of Pods. 
## 2. Key Functions of a Service
* **Stable Networking**: Provides a permanent entry point (IP and port) that is not connected to the lifecycle of the Pod.
* **Load Balancing**: Acts as a load balancer to distribute traffic among the Pods it targets.
* **Loose Coupling**: Decouples the frontend or calling service from the specific IP addresses of the backend Pods.
* **Service Discovery**: Allows components within the cluster (like a web app and a database) to communicate reliably using the Service's stable IP.

## 3. Connecting Services to Pods
Services use **Labels and Selectors** to identify which Pods they should manage:
* **Labels**: Key-value pairs attached to Pods (e.g., `app: my-app`) to serve as identifying attributes.
* **Selectors**: The Service uses a selector to find and route traffic to all Pods that have matching labels.
* **Ports**: 
    * **Port**: The port where the Service itself is accessible.
    * **TargetPort**: The port on the container where the traffic is actually sent (must match the port the container is listening on).

## 4. Internal vs. External Services

* **Internal Service**: The default setting (ClusterIP). It is used for components like databases that should not be accessible from outside the cluster.
* **External Service**: Used when an application needs to be accessible via a browser or external client.

| Type             | Description                                                                                      | Use Case                                                     |
| :--------------- | :----------------------------------------------------------------------------------------------- | :----------------------------------------------------------- |
| **ClusterIP**    | The default type. Exposes the Service on a cluster-internal IP(default)                          | Internal communication (e.g., databases).                    |
| **NodePort**     | Exposes the Service on each Node's IP at a static port.                                          | Basic external access (not recommended for production).      |
| **LoadBalancer** | [cite_start]Exposes the Service externally using a cloud provider's load balancer                | Standard way to expose services to the internet in the cloud |
| **Headless**     | A ClusterIP service where you want to talk to a specific Pod directly instead of load balancing. | Stateful applications like databases.                        |
!<img src="../../../diagram.png">
Ingress, loadbalancer type: expose to external
- loadbalancer: use loadbalance of cloud provider( cost more money)
- Ingress use k8s native support


---
Best Practice: 
- Do NOT use NodePort Service Type for external connections. Use Ingress or Load Balancer instead.
- Service: https://kubernetes.io/docs/concepts/services-networking/service/

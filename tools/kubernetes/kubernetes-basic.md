# Kubernetes Basic

Kubernetes features

* **Automatic bin packing**
* **Self healing**
  * It kills and restarts containers unresponsive to the health checks.
  * Prevent traffic to unresponsive containers.
* **Horizontal scaling**
* **Service discovery and Load balancing**
* **Automated rollouts and rollbacks**
* **Secret and configuration management**
* **Storage orchestration**
* **Batch execution**

Cluster

Kubernetes has:

* control plane - one or more master nodes
* worker nodes

### Control Plane components

Managing a state of Kubernetes cluster. Make a global decision about the cluster.

Losing the control plane may introduce downtime.

To ensure **control plane fault tolerance** mater node can add instance in **High-Availability mode.**

Can be run on any node in the cluster. For simplicity, all control plane components are located on the separated machine.

* **kube-apiserver**
  * read/update cluster state from etcd data store
  * only component to talk with etcd
  * middle interface for any other control plane agent
* **etcd**
  * you can only add new data , data is never replaced
  * **etcdctl -**
* **kube-scheduler**
  * assign pods to nodes
  * decisions are made based on current state and new object requirenments
* kube-controller-manager - runs controller processes.
  * Node controller - noticing and responding when nodes go down.
  * Job controller - watch for jobs, create Pods to complete the job.
  * Endpoints controller - Populates the Endpoints object (that is, joins Services & Pods).
  * Service account and Token controllers - Create default accounts and API access tokens for new namespaces.
* cloud-controller-manager - cloud specific control logic.
  * Node controller - For checking the cloud provider to determine if a node has been deleted in the cloud after it stops responding
  * Route controller - For setting up routes in the underlying cloud infrastructure
  * Service controller - For creating, updating and deleting cloud provider load balancers

### Node components

Node components run on every node, maintaining running pods and providing the Kubernetes runtime environment.

* container runtime
  * Docker
  * CRI-O
  * containerd
  * frakti
* kubelet - an agent that runs on each node.
  * communicate with control plane components
* kube-proxy - network rules in the node.
* Addons
  * **DNS**
  * **Dashboard**
  * **Monitoring**
  * **Logging**

### Networking

* container to container inside pod
  * network namespace - can be shared across containers
  * inside the pod container can talk through the localhost
* pod-to-pod on the same node and across the cluster
* pod-to-service
* external-to-service

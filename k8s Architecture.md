KUBERNETES ARCHITECTURE

1. CONTROL PLANE (Master Node)
-------------------------------------
| API Server         | Manages communication with the cluster.
| Scheduler          | Assigns pods to worker nodes.
| Controller Manager | Ensures the cluster's desired state is maintained.
| etcd               | Stores configuration and state information.

2. WORKER NODES
-------------------------------------
| Kubelet            | Ensures pods are running and reports back to the master.
| Kube Proxy         | Handles networking and routing to the correct pods.
| Pods               | The smallest unit, running one or more containers.

3. NETWORKING
-------------------------------------
| Flat Network       | Enables all pods to communicate with each other.
| Services           | Provide stable endpoints for accessing pods.

COMMUNICATION FLOW:
-------------------------------------
Control Plane ⇔ Worker Nodes ⇔ Pods

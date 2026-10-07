${\color{red} \textbf{Persistent Volume}}$

${\color{red} \textbf{Persistent Volume Claim}}$

${\color{red} \textbf{Statefulset}}$

## StatefulSet

StatefulSet is used to manage **stateful applications** in Kubernetes.

- Provides **unique identity** to each pod  
- Maintains **stable hostname and storage**  
- Pods are created and deleted **in order (sequentially)**  

---

### Key Features

- **Stable Pod Names**
  → pod-0, pod-1, pod-2 (fixed identity)

- **Persistent Storage**
  → Each pod gets its own PersistentVolume  
  → Data is not lost after restart  

- **Ordered Deployment**
  → Pods start one by one (0 → N)  
  → Termination happens in reverse order  

- **Stable Network Identity**
  → Each pod has a fixed DNS name  

---

### Use Cases

- Databases (MySQL, MongoDB)  
- Distributed systems (Kafka, Zookeeper)  
- Applications requiring persistent data  

---

| Feature              | Deployment                        | StatefulSet                           |
| -------------------- | --------------------------------- | ------------------------------------- |
| **Application Type** | Stateless apps                    | Stateful apps                         |
| **Pod Identity**     | Interchangeable                   | Unique identity (fixed)               |
| **Pod Naming**       | Random names                      | Fixed names (`pod-0`, `pod-1`)        |
| **Storage**          | No persistent storage (ephemeral) | Persistent storage (PVC for each pod) |
| **Scaling**          | Fast and unordered                | Ordered scaling (0 → n)               |
| **Pod Creation**     | Parallel                          | Sequential                            |
| **Pod Deletion**     | Random                            | Reverse order                         |
| **Data Handling**    | Data can be lost                  | Data is preserved                     |
| **Use Cases**        | Web apps, APIs                    | Databases, Kafka, Zookeeper           |

---


````
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: nginx-statefulset
spec:
  selector:
    matchLabels:
      app: nginx
  serviceName: "nginx"
  replicas: 2
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
          name: web
        volumeMounts:
        - name: nginx-persistent-storage
          mountPath: /usr/share/nginx/html
  volumeClaimTemplates:
  - metadata:
      name: nginx-persistent-storage
    spec:
      accessModes: ["ReadWriteOnce"]
      resources:
        requests:
          storage: 1Gi
---
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-nginx
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-nginx
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
````

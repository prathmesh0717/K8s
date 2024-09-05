
## K8s Volume types:
1. EmptyDir
2. HostPath
3. PersistentVolume
4. PersistanceVolumeClaim

1. EmptyDir
   
```yaml
apiVersion: v1
kind: Pod
metadata: 
 name: pod-1

spec:
  containers:
    - name: cont1
      image: nginx
      volumeMounts:
      - name: ebs
        mountPath: /tmp/data

  volumes: 
  - name: ebs
    emptyDir:
      sizeLimit: 500Mi
```
# ${\color{red} \textbf{Volume}}$

${\color{green} \textbf{EmptyDir}}$
````
apiVersion: v1 
kind: Pod
metadata: 
  name: pod-1
spec: 
  containers:
  - image: nginx
    name: cont-1
    volumeMounts: 
      - mountPath: /data
        name: vol-1
  volumes:
  - name: vol-1
    emptyDir: {}
````
When mounts the /data folder in that pod then when our container is deleted hence pod create a new container automatically that time in the new container /data have our previous data of old container bcoz we mount that folder and its volume is connected with pod not container.


${\color{green} \textbf{HostPath}}$
````
apiVersion: v1 
kind: Pod
metadata: 
  name: pod-1
spec: 
  containers:
  - image: nginx
    name: cont-1
    volumeMounts: 
      - mountPath: /data  #it path in the pod.
        name: vol-1
  volumes:
  - name: vol-1
    hostPath:
      path: /tmp/data  #it is a local path in node.
````

In that case our volume is stored on worker node thats why when our node is terminated and newly created that time volume is attached with newly created pod.

we mounted the /data folder when pod deleted and its newly created that time newly pod have old pod data which was have in /data folder not have any other directory old data which we created in that.

**Master**
1. kubectl apply -f hostpath.yml
2. kubectl exec -it pod-1 --bash
3. cd /data

![image](https://github.com/user-attachments/assets/9b2b2d41-1f17-46c6-8f1b-79bd40fed182)

**open a copy of vm in new window**

**Worker Node**
4. minikube ssh 
5. cd /tmp/data

![image](https://github.com/user-attachments/assets/d0a773a9-b13a-4edf-8e81-d0fa20b73b09)

**Output :** 
- which we add the files or any data in /data folder in pod that data automatically available in Worker node in the hostpath /tmp/data.
- when the pod is terminated and newly created hence our old volume monted /data attached with newly created pod automatically. 


${\color{green} \textbf{Persistent Volume and Persistent Volume Claim}}$

````
#pvc
apiVersion: v1
kind: PersistentVolumeClaim
metadata: 
  name: pvc
spec: 
 storageClassName: ""
 accessModes: 
 - ReadWriteOnce  #you only need one pod to write to the volume at any time
 resources:
   requests:
      storage: 10Gi
#pv
---
apiVersion: v1
kind: PersistentVolume
metadata: 
  name: pv
spec: 
 storageClassName: ""
 accessModes: 
 - ReadWriteOnce
 capacity:          #It Define the amount of storage
      storage: 10Gi
 hostPath: 
   path: "/tmp/data"  #path of host node
 persistentVolumeReclaimPolicy: Recycle  #policy for pv recyle means we use that pv with another pvc also.

#deployment with pvc
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: nginx:latest
        volumeMounts:
        - mountPath: "/tmp/data"  #mounted path for pod, which available in pod.
          name: vol-1  #our volume name.
      volumes:
      - name: vol-1  
        persistentVolumeClaim:
          claimName: pvc1   #our pvc name
````

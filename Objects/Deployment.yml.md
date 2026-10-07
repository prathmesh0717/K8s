${\color{red} \textbf{Deployment}}$

````
apiVersion: apps/v1
kind: Deployment
metadata:
  name: deployment-1
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template: 
    metadata:
      labels:
        app: my-app
    spec: 
      containers: 
      - name: cont1
        image: httpd:latest
        ports:
        - containerPort: 80
````
## 🚀 Deployment Strategies in Kubernetes

🔗 Reference: [https://medium.com/@ezekiel.umesi/understanding-deployment-strategies-in-kubernetes-3e447d060280](https://medium.com/@ezekiel.umesi/understanding-deployment-strategies-in-kubernetes-3e447d060280)

---

### 1. Recreate Strategy

```
All existing pods are terminated first, then new pods are created.

- Simple but causes downtime ❌
- Application is unavailable during update
- Suitable for non-critical apps
```

---

### 2. Rolling Update Strategy (Default ✅)

```
Pods are updated one by one without downtime.

- New pods are created gradually
- Old pods are terminated step by step
- Ensures zero downtime ✔️

Key Config:
maxSurge: 1        → 1 extra pod can be created
maxUnavailable: 1  → 1 pod can be down

👉 Flow: +1 new pod → -1 old pod → repeat
```

---

### 3. Blue/Green Deployment

```
Two environments: Blue (current) and Green (new)

- Deploy new version in parallel (Green)
- Test it before switching traffic
- Switch traffic using Service

✔️ No downtime
✔️ Easy rollback
❌ Requires more resources
```

---

### 4. Canary Deployment

```
New version is released to a small set of users first.

- Gradual rollout
- Monitor before full release
- Reduces risk in production

✔️ Safe deployment
✔️ Real user testing
```
---

##  Useful Commands

```bash
kubectl get pods                          # Check pods
kubectl apply -f deployment.yaml          # do changes and see difference 
kubectl get pods -w                       # to watch 
kubectl describe deployment my-app        # Detailed info - 
kubectl rollout status deployment/my-app  # Rollout status -
kubectl rollout history deployment/my-app # Version history -
kubectl rollout undo deployment/my-app    # Rollback
```
```
kubectl set image deployment/my-app my-container=nginx:latest    # Change image without editing YAML
kubectl scale deployment my-app --replicas=5                     # Scale deployment
```
---

 **Quick Summary:**

* Recreate → downtime
* Rolling → zero downtime (default)
* Blue/Green → switch traffic
* Canary → gradual release

---


````
apiVersion: apps/v1
kind:  Deployment
metadata: 
  name: deploy
  labels:
    app: depapp
spec:
    strategy:
      type: RollingUpdate
      rollingUpdate:
            maxSurge: 1     # Maximum number of pods that can be created over the desired replicas
            maxUnavailable: 1  # Maximum number of pods that can be unavailable during the update
    selector: 
      #matchExpressions:      
         #  -  { key: app, operator: In , values: [rsapp,  new-app]}
         #   -  { key: app, operator: Exists}
      matchLabels: 
          app: depapp
    replicas: 10
    template:
      metadata:
        labels:
          name: nginxapp
          app: depapp
      spec:
        containers:
          - name: nginxapp
            image: httpd:latest 
            ports:
                - containerPort: 80
                  protocol: TCP
````

````
apiVersion: apps/v1
kind: Deployment
metadata:
 name: my-app
spec:
 replicas: 5
 selector:
   matchLabels:
     app: mario-app
 minReadySeconds: 5
 strategy:
  type: RollingUpdate
  rollingUpdate:
    maxUnavailable: 1
    maxSurge: 1
 template:
   metadata:
     labels:
       app: mario-app
   spec:
    containers:
     - name: mario-cont1
       image: mukunddeo9325/super-mario
       ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
 name: myapp-service
spec:
 type: NodePort
 selector:
   app: mario-app
 ports:
  - protocol: TCP
    port: 80
    targetPort: 80

````

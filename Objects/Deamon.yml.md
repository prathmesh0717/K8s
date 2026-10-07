![image](https://github.com/user-attachments/assets/a73a3587-0f83-4769-928f-e2bb150fc77d)

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: app1
spec:
  selector:
    matchLabels:
      app: app1
  template:
    metadata:
      labels:
        app: app1
    spec:
      containers:
        - name: app1-container
          image: nginx
          ports:
            - containerPort: 80
```

## Deamonset
- A Kubernetes DaemonSet ensures a specific pod runs on all (or a subset of) nodes in a cluster. As nodes are added or removed, the DaemonSet automatically adds or removes the required pods, making it ideal for background tasks like logging agents, monitoring, and network plugins.

```deamonset.yaml
apiVersion: apps/v1
kind:  DaemonSet
metadata: 
  name: daemon
  labels:
    app: depapp
spec:
    selector: 
      matchLabels: 
          app: depapp
    template:
      metadata:
        labels:
          name: cwatch
          app: depapp
      spec:
        containers:
          - name: cwatch
            image: amazon/cloudwatch-agent:latest
            ports:
                - containerPort: 80
                  protocol: TCP
```

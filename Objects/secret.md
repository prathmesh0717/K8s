### ${\color{red} \textbf{Secret}}$

````
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
type: Opaque
data:
  username: dXNlcm5hbWU=  # base64 encoded value of 'username'
  password: cGFzc3dvcmQ=  # base64 encoded value of 'password'
---
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: my-container
    image: nginx
    env:
    - name: SECRET_USERNAME
      valueFrom:
        secretKeyRef:
          name: my-secret
          key: username
    - name: SECRET_PASSWORD
      valueFrom:
        secretKeyRef:
          name: my-secret
          key: password
  restartPolicy: Never
````

````
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app-deployment

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
        - name: app-container
          image: nginx:latest

          ports:
            - containerPort: 80

          volumeMounts:
            # ConfigMap volume
            - name: config-volume
              mountPath: /etc/app/config

            # Secret volume
            - name: secret-volume
              mountPath: /etc/app/secret

      volumes:
        # Attach ConfigMap
        - name: config-volume
          configMap:
            name: app-config

        # Attach Secret
        - name: secret-volume
          secret:
            secretName: app-secret
````

 🔹 ConfigMap vs Secret in Kubernetes

##  Difference Table

| Feature | ConfigMap | Secret |
|--------|----------|--------|
| Purpose | Store non-sensitive data | Store sensitive data |
| Data Type | Plain text | Base64 encoded |
| Use Case | App configs, env variables | Passwords, tokens, API keys |
| Security | Not secure | More secure (can enable encryption) |
| Storage | Stored as plain text in etcd | Stored encoded in etcd |
| Access Control | Basic | Use RBAC for strict control |
| Example | DB host, app config | DB password, API token |

---

##  Key Notes

###  ConfigMap Notes
1. **Non-Confidential Data:** Do not store sensitive data.
2. **Dynamic Updates:** Can update without restarting Pods (if mounted as volume).
3. **Lightweight:** Avoid storing large or complex data.

###  Secret Notes
1. **Secure Handling:** Avoid plain text files; use `kubectl`.
2. **Encryption:** Enable encryption at rest for better security.
3. **Access Control:** Use RBAC to restrict access.

---

## ⚡ Quick Summary

- **ConfigMap → Non-sensitive configuration**
- **Secret → Sensitive/secure data**

---


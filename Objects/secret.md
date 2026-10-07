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

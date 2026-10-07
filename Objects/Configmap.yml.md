### ${\color{red} \textbf{ConfigMap}}$

````
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-config
data:
  key1: value1
  key2: value2
---
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: my-container
    image: nginx
    volumeMounts:
    - name: config-volume
      mountPath: /etc/config
  volumes:
  - name: config-volume
    configMap:
      name: my-config
  restartPolicy: Never
````

````
apiVersion: v1 
kind: ConfigMap
metadata:
    name: links-cm
data:
 url: "https://templatemo.com/download/templatemo_632_machina"
````

````

apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_NAME: "MyApplication"
  APP_ENV: "development"
  APP_PORT: "8080"
  DB_HOST: "mysql-service"
  DB_PORT: "3306"
---
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: app-container
      image: nginx
      envFrom:
        - configMapRef:
            name: app-config
      volumeMounts:
        - name: config-volume
          mountPath: /etc/myapp

  volumes:
    - name: config-volume
      configMap:
        name: app-config
````

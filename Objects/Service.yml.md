# ${\color{blue} \textbf{Service}}$

## ${\color{green} \textbf{ClusterIP}}$

${\color{orange} \textbf{Deployment and Service 1}}$


````
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app1
spec:
  replicas: 1
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

---
apiVersion: v1
kind: Service
metadata:
  name: app1-service
spec:
  selector:
    app: app1
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
````

${\color{cyan} \textbf{Deployment and Service 2}}$
````
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app2
spec:
  replicas: 1
  selector:
    matchLabels:
      app: app2
  template:
    metadata:
      labels:
        app: app2
    spec:
      containers:
        - name: app2-container
          image: tomcat
          ports:
            - containerPort: 8080

---
apiVersion: v1
kind: Service
metadata:
  name: app2-service
spec:
  selector:
    app: app2
  ports:
    - protocol: TCP
      port: 8080
      targetPort: 8080
  type: ClusterIP
````


## ${\color{brown} \textbf{NodePort Ip Range 30000 To 32767}}$

````
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app1
spec:
  replicas: 1
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

---
apiVersion: v1
kind: Service
metadata:
  name: app1-service
spec:
  type: NodePort
  selector:
    app: app1
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 30001  # You can specify a particular port or let Kubernetes allocate one for you
````

## ${\color{blue} \textbf{Load Balancer}}$

````
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app1
spec:
  replicas: 1
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

---
apiVersion: v1
kind: Service
metadata:
  name: app1-service
spec:
  type: LoadBalancer
  selector:
    app: app1
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
````

## ${\color{orange} \textbf{External Name}}$
````
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app1
spec:
  replicas: 1
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

---
apiVersion: v1
kind: Service
metadata:
  name: app1-external-service
spec:
  type: ExternalName
  externalName: example.com
````

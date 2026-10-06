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

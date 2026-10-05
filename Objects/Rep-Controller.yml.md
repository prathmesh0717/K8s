${\color{green} \textbf{r-controller.yaml file}}$
````
apiVersion: v1
kind: ReplicationController
metadata:
 name: rc-demo
 labels:
  env: dev

spec:
 replicas: 3
 selector:
    env: dev
 template:
   metadata:
    labels:
      env: dev

   spec:
    containers:
    - name: demo
      image: nginx
      ports:
      - containerPort: 80
````

${\color{red} \textbf{Labels :}}$  Key-value pairs assigned to Kubernetes objects for organization and selection.

${\color{red} \textbf{Replicas :}}$  The desired number of identical pod instances to run.

${\color{red}  \textbf{Selector :}}$ Criteria used to identify and manage groups of labeled objects.

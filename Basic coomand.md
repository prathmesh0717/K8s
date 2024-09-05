# $${\color{violet} \textbf{Basic Commands of k8s}}$$

## ${\color{cyan} \textbf{pod}}$

1. Check the minikube install or not.
````
kubectl get node
````
2. Create pod.
````
kubectl apply -f <file.yaml>
````
3. Listing of pods
````
kubectl get pod
````
4. All info of particular pod.
````
kubectl describe pod <pod_name>
````
5. Show wide info of pods.
````
kubrctl get pods -o wide
````
6. Delete the pod.
````
kubectl delete pod <pod_name>
````
7. Enter the pod
````
kubectl exec <pod_name>  -it -c <container_name> -- /bin/bash
````
8. create a pod with command.
````
kubectl run nginx --image=nginx --restart=Never
````
9. All delete which created with yaml file
````
kubectl delete -f <file.yml>
````
10. tp terminate the container in pod.
````
service stop nginx
````
## ${\color{cyan} \textbf{Replication Controller}}$


1. Create Replication controller
````
kubectl apply -f rc.yaml
````
2. List of Replication controller
````
kubectl get rc
````
3. Delete Replication Controller
````
kubectl delete rc <rc-name>
````

## ${\color{cyan} \textbf{Replica Set}}$

1. Create Replica set
````
kubectl appply -f rs.yaml
````
2. List of Replica Set.
````
kubectl get rs
````
3. Delete Replica set.
````
kubectl delete rs <rs-name>
````

## ${\color{cyan} \textbf{Deployment}}$

1. Create Deployment wit command
````
kubectl create deployment <deploy_name> --image=nginx --replicas=2
````
2. Convert yaml language which deployment created with command in particular file
````
kubectl get deployment -o yaml > <file.yaml>
````
3. list of deployment
````
kubectl get deployment
````
4. Delete the Deployment
````
kubectl delete deployment <deployment_name>
````
5. view the history of deployments, including revisions and changes
````
kubectl rollout history deployment <deployment_name>
````
6. Update the image Version.
````
kubectl set image deployment/<deployment_name> <container_name>=<new_version> --record
````
7. Get all info about which we created.
````
kubectl get all
````
8. Show the brief information about particular deployment.
````
kubectl get deployment <deployment_name> -o wide
````
9. To increase pod replicas in existing Deployment.
````
kubectl scale deployment <deployment_name> --replicas=<number>
````
10. show the revision version history of deployment.
````
kubectl rollout history deployment <deployment_name>
````
11. Rollback current version to recent version.
````
kubectl rollout undo deployment <deployment_name>
````
12. rollback to particular version using revision number.
````
kubectl rollout undo deployment <deployment_name> --to-revision=<number_of_revision>
````
13. Confirmation to Rollout.
````
kubectl rollout status deployment <deployment_name>
````
## ${\color{cyan} \textbf{Namespace}}$

1. Create a namespace.
````
kubectl create ns <ns_name>
````
2. Delete Namespace.
````
kubectl delete ns <ns_name>
````
3. List of Namespaces.
````
kubectl get ns
````
4. Get all pods with all namespaces.
````
kubectl get pods --all-namespaces
````
5. Get pod list in particular namespaces.
````
kubectl get pod -n <ns_name>
````
6. Set default Namespace.
````
kubectl config set-context --current --namespace=<ns_name>
````
7. apply any resource in particular name space.
````
kubectl apply -f <file.yaml> --namespace <ns_name>
````
8. Ceck the default Namespace.
````
kubectl config view --minify | grep namespace:
````
9. Run command in the particular Namespace.
````
kubectl exec -it <pod_name> -n <ns_name> -- <command which we want to run>
````
## ${\color{cyan} \textbf{Service}}$

1. Create Service.
````
kubectl expose deployment <deployment-name> --type=ClusterIP --port=<port> --target-port=<target-port> --name=<service-name>

````
2. Communicate between services from pod.
````
curl <opposite_service_ip>:<port_of_pod>
````
3. List of service.
````
kubectl get svc
````
4. Describe service.
````
kubectl describe service <service-name>
````
5. Delete Service.
````
kubectl delete service <service-name>
````
6. Edit Service.
````
kubectl edit service <service-name>
````
7. Get Service Details in a Specific Namespace.
````
kubectl get services -n <namespace>
````
8. Get endpoints of a service.
````
kubectl get endpoints <service-name>
````
## ${\color{cyan} \textbf{Daemonset}}$

1. List Daemonset.
````
kubectl get daemonsets
````
2. Describe Daemonset.
````
kubectl describe daemonset <daemonset-name>
````
3. Delete Daemonset.
````
kubectl delete daemonset <daemonset-name>
````
4. Edit a Daemonset
````
kubectl edit daemonset <daemonset-name>
````
5. Get Daemonset Pods.
````
kubectl get pods -l name=<daemonset-name>
````
6. Scale a DaemonSet
````
kubectl scale daemonset <daemonset-name> --replicas=<number-of-replicas>
````
## ${\color{cyan} \textbf{Statefulset}}$

1. List of sts.
````
kubectl get statefulsets
````
2. Describe statefulset
````
kubectl describe statefulset <statefulset-name>
````
3. Delete Statefulset
````
kubectl delete statefulset <statefulset-name>
````
## ${\color{cyan} \textbf{ConfigMap}}$

1. Create Configmap from file.
````
kubectl create configmap <configmap-name> --from-file=<path-to-file>
````
2. Create a ConfigMap with Multiple file.
````
kubectl create configmap <configmap-name> --from-file=<path-to-file1> --from-file=<path-to-file2>
````
3. Create a ConfigMap from literal values.
````
kubectl create configmap <configmap-name> --from-literal=<key1>=<value1> --from-literal=<key2>=<value2>
````
4. Create a configmap from Directory.
````
kubectl create configmap <configmap-name> --from-file=<directory-path>
````
5. List of Configmap.
````
kubectl get configmaps
````
6. Describe a ConfigMap.
````
kubectl describe configmap <configmap-name>
````
7. Delete a Configmap.
````
kubectl delete configmap <configmap-name>
````
8. View the data in ConfigMap.
````
kubectl get configmap <configmap-name> -o yaml
````
9. List all keys in Configmap.
````
kubectl get configmap <configmap-name> -o jsonpath='{.data}' | jq -r 'keys[]'
````
## ${\color{cyan} \textbf{Secret}}$

1. Create a Secret from Literal Values
````
kubectl create secret generic <secret-name> --from-literal=<key1>=<value1> --from-literal=<key2>=<value2>
````
2. Create a Secret from a File
````
kubectl create secret generic <secret-name> --from-file=<key>=<path-to-file>
````
3. Create a Secret from Multiple Files
````
kubectl create secret generic <secret-name> --from-file=<path-to-file1> --from-file=<path-to-file2>
````
4. Create a Secret for Docker Registry
````
kubectl create secret docker-registry <secret-name> \
  --docker-username=<username> \
  --docker-password=<password> \
  --docker-email=<email>
````
5. Create a TLS Secret
````
kubectl create secret tls <secret-name> --cert=<path-to-cert-file> --key=<path-to-key-file>
````
6. List all Secret
````
kubectl get secrets
````
7. Describe Secret
````
kubectl describe secret <secret-name>
````
8. Delete Secret.
````
kubectl delete secret <secret-name>
````
9. View a Secret in YAML Format
````
kubectl get secret <secret-name> -o yaml
````
10. View the Data in a Secret
````
kubectl get secret <secret-name> -o jsonpath='{.data}'
````

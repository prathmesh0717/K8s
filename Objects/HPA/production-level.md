## Namespace

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: production
  labels:
    app.kubernetes.io/part-of: nginx-production
```
## HPA with Deployment File
```yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deploy
  namespace: production
  labels:
    app: nginx
spec:
  replicas: 2
  revisionHistoryLimit: 5
  progressDeadlineSeconds: 600

  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0

  selector:
    matchLabels:
      app: nginx

  template:
    metadata:
      labels:
        app: nginx
    spec:
      automountServiceAccountToken: false
      terminationGracePeriodSeconds: 30

      securityContext:
        runAsNonRoot: true
        runAsUser: 101
        runAsGroup: 101
        seccompProfile:
          type: RuntimeDefault

      affinity:
        podAntiAffinity:
          preferredDuringSchedulingIgnoredDuringExecution:
            - weight: 100
              podAffinityTerm:
                topologyKey: kubernetes.io/hostname
                labelSelector:
                  matchLabels:
                    app: nginx

      containers:
        - name: nginx
          image: nginxinc/nginx-unprivileged:stable-alpine
          imagePullPolicy: Always

          ports:
            - name: http
              containerPort: 8080
              protocol: TCP

          resources:
            requests:
              cpu: 100m
              memory: 64Mi
            limits:
              cpu: 500m
              memory: 128Mi

          startupProbe:
            httpGet:
              path: /
              port: http
            periodSeconds: 5
            failureThreshold: 30

          readinessProbe:
            httpGet:
              path: /
              port: http
            periodSeconds: 5
            timeoutSeconds: 2
            failureThreshold: 3

          livenessProbe:
            httpGet:
              path: /
              port: http
            periodSeconds: 10
            timeoutSeconds: 2
            failureThreshold: 3

          securityContext:
            allowPrivilegeEscalation: false
            readOnlyRootFilesystem: true
            capabilities:
              drop:
                - ALL

          volumeMounts:
            - name: nginx-cache
              mountPath: /var/cache/nginx
            - name: nginx-tmp
              mountPath: /tmp

      volumes:
        - name: nginx-cache
          emptyDir: {}
        - name: nginx-tmp
          emptyDir: {}
````
## HPA with LoadBalancer Service
````yaml

apiVersion: v1
kind: Service
metadata:
  name: nginx-service
  namespace: production
  labels:
    app: nginx
spec:
  type: LoadBalancer
  selector:
    app: nginx
  ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: http
````
## HPA File
````yaml

apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: nginx-hpa
  namespace: production
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: nginx-deploy

  minReplicas: 2
  maxReplicas: 5

  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 70

  behavior:
    scaleUp:
      stabilizationWindowSeconds: 0
      selectPolicy: Max
      policies:
        - type: Pods
          value: 2
          periodSeconds: 60
        - type: Percent
          value: 100
          periodSeconds: 60

    scaleDown:
      stabilizationWindowSeconds: 300
      selectPolicy: Min
      policies:
        - type: Pods
          value: 1
          periodSeconds: 60
```
## HPA pdb.yml Pod Disruption Budget

# This helps maintain at least one available pod during supported voluntary disruptions, such as node maintenance.

# A PodDisruptionBudget does not protect against every outage, such as a node crashing unexpectedly.

````yaml

apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: nginx-pdb
  namespace: production
spec:
  minAvailable: 1
  selector:
    matchLabels:
      app: nginx
````

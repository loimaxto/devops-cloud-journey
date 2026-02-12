### Summary
- How to config service, deployment, secrete, configmap
### Requirements

#### Workflow
![](demo_workflow.png)

### Setup

### Command
```bash
echo -n "username" | base64 # decode
kubectl apply -f secrete-file.yaml
kubectl get secret
kubectl delete secret/deployment/service <name>
kubectl describe service/pod
kubectl get all
kubectl get pod/service/....
kubectl get replicaset

minikube tunnel
minikube service <name>
```

#### 1 Mange secrete

```bash
echo -n "username" | base64 # decode
kubectl apply -f secrete-file.yaml
kubectl get secret
```

**Secret file**
```yaml
apiVersion: v1
kind: Secret
metadata: 
  name: dm-mongodb-secret
type: Opaque
data:
  mongo-root-username: dXNlcg==
  mongo-root-password: MTIzNA==
```

#### 2 Mongodb file

```yaml
apiVersion: apps/v1 
kind: Deployment
metadata:
  name: dm-mongodb-deployment
  labels:
    app: mongodb
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mongodb
  template: 
    metadata:
      labels:
        app: mongodb
    spec:
      containers:
      - name: mongodb
        image: mongo:6.0-jammy
        ports:
        - containerPort: 27017
        env:
        - name: MONGO_INITDB_ROOT_USERNAME
          valueFrom:
            secretKeyRef:
              name: dm-mongo-secret
              key: mongo-root-username
        - name: MONGO_INITDB_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: dm-mongo-secret
              key: mongo-root-password
        # ping to app in pod to check health
		livenessProbe:
			grpc: # check protocal, depends app
				port: 27017
			periodSeconds: 5
		# ping to check start pod only
		readinessProbe:
			grpc:
				port: 27017
			periodSeconds: 5
		resources:
			requests:
				cpu: 100m
				memory: 64Mi 
			limits:
				cpu: 200m
				memory: 80Mi
---
apiVersion: v1
kind: Service
metadata:
  name: mongodb-service
spec:
  selector:
    app: mongodb
  ports:
  - protocol: TCP
    port: 27017
    targetPort: 27017 
```

#### 3 Mongo express file

```yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongo-express
  labels:
    app: mongo-express
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mongo-express
  template:
    metadata:
      labels:
        app: mongo-express
    spec: 
      containers:
      - name: mongo-express
        image: mongo-express
        ports:
        - containerPort: 8081
        env:
        - name: ME_CONFIG_MONGODB_ADMINUSERNAME
          valueFrom: 
            secretKeyRef:
              name: mongodb-secret
              key: mongo-root-username
        - name: ME_CONFIG_MONGODB_ADMINPASSWORD
          valueFrom:
            secretKeyRef:
              name: mongodb-secret
              key: mongo-root-password
        - name: DATABASE_URL
          valueFrom:
            configMapKeyRef:
              name: mongodb-configmap
              key: database_url
        - name: ME_CONFIG_MONGODB_URL
          value: "mongodb://$(ME_CONFIG_MONGODB_ADMINUSERNAME):$(ME_CONFIG_MONGODB_ADMINPASSWORD)@$(DATABASE_URL)"
---
apiVersion: v1
kind: Service
metadata:
  name: mongo-express-service
spec:
  selector:
    app: mongo-express
  type: LoadBalancer
  ports:
  - protocol: TCP
    port: 8081
    targetPort: 8081
    nodePort: 30000
```
#### 4 configMap
change config map will auto change value of binding service
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: mongodb-configmap
data:
  database_url: "mongodb-service:27017"
```

### Prerequisite
- Have Digital Ocean account
- Install kubectl
### Introduce

In this project, I will deploy statefulset mongodb using helm chart to Kubernetes Digital Ocean(DOKS)
1. Setup Helm charts
2. Create k8s cluster on DOKS
3. Deploy mongodb
4. Deploy mongo express to monitor mongodb
5. Connect to Ingress and config access from internet

### 1 Create Digital Ocean K8s cluster and get connection file

Creat K8S cluster
![](screenshots/Pasted%20image%2020260205110158.png)
Down load config cluster file: 
![](screenshots/Pasted%20image%2020260205110402.png)




Export k8s-connect file path to kubectl so it will interact with k8s cluster of DO

```bash
export KUBECONFIG=~/Desktop/mycluster-config.yaml
kubectl get nodes
```
At this stage, kubectl will connect to DO cluster
![](screenshots/Pasted%20image%2020260205110627.png)
### 2 Setup Helm chart
```bash
sudo apt-get install curl gpg apt-transport-https --yes
curl -fsSL https://packages.buildkite.com/helm-linux/helm-debian/gpgkey | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/helm.gpg] https://packages.buildkite.com/helm-linux/helm-debian/any/ any main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm

```
**Add bitnami helm repo**
```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm search repo bitnami/name-rop
help repo update
```


### 3 Create config files
##### helm-mongodb
1. Connects to DOKS
2. Create physical storage
3. Attach to Pods

Helm automically create pods, services, storage template so we don't need to setup things manually

```yaml
architecture: replicaset
replicaCount: 2
persistence:
  storageClass: "do-block-storage"
auth:
  rootPassword: secret-root-pwd
image:
  registry: docker.io
  tag: latest
  repository: bitnamisecure/mongodb
global:
  security:
    allowInsecureImages: True
metrics:
  enabled: false
```


**Execute command**: this will get value from the file above, automically setup service on DOKS.
```bash
helm install mongodb --values helm-mongodb.yaml bitnami/mongodb
```
**Result:**
It will create services, pods
```bash
kubectl get all
```

![](screenshots/Pasted%20image%2020260205112545.png)


get secret
![](screenshots/Pasted%20image%2020260205112949.png)

![](screenshots/mongo_secret.png)
##### mongo-express

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
          value: root
        - name: ME_CONFIG_MONGODB_ADMINPASSWORD 
          valueFrom:
            secretKeyRef:
              name: mongodb
              key: mongodb-root-password
        - name: ME_CONFIG_MONGODB_URL
          value: "mongodb://$(ME_CONFIG_MONGODB_ADMINUSERNAME):$(ME_CONFIG_MONGODB_ADMINPASSWORD)@mongodb-0.mongodb-headless:27017"
---
apiVersion: v1
kind: Service
metadata:
  name: mongo-express-service
spec:
  selector:
    app: mongo-express
  ports:
    - protocol: TCP
      port: 8081
      targetPort: 8081

```


![](screenshots/Pasted%20image%2020260205112731.png)
##### helm-ingress
add ingress loadbalancer
![](screenshots/Pasted%20image%2020260205113804.png)


![](screenshots/Pasted%20image%2020260205113903.png)

![](screenshots/Pasted%20image%2020260205114502.png)

create host domain for k8s
![](screenshots/Pasted%20image%2020260205115627.png)


```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
    annotations:
      kubernetes.io/ingress.class: nginx
    name: mongo-express
spec:
  rules:
  - host: k8s-test.com
    http:
      paths:
        - path: /
          pathType: Prefix
          backend:
            service:
              name: mongo-express-service
              port:
                number: 8081

```

**Resutl**:

![](screenshots/Pasted%20image%2020260206115358.png)

### Remove resources

```bash
helm uninstall mongodb
kubectl get pod
```
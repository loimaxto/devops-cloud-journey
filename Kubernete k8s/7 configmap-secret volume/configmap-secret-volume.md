
# Summary
- Deploying message broker mosquitto, 
- Knowing how to use configmap, secret, volume
When config map update, app in pod will update without recreate
### Commands
```bash
kubectl get configmap

kubectl get secret
kubectl get pod
kubectl apply -f 
kubectl exec -it podname -- /bin/sh
```


## Prepare

#### 1 Secret file
```yaml
apiVersion: v1
kind: Secret
metadata:
    name: mosquitto-secret-file
type: Opaque
data:
	# base64 encode
    secret.file: |
        VGVjaFdvcmxkMjAyMyEgLW4K
```

#### 2 Configmap file
- **How it Connects:**
    - **Environment Variables:** Values are injected into the container at startup.
    - **Volumes:** Config data appears as **files** inside the container’s folder.
- **Updating Behavior:**
    - **Files (Volumes):** Update automatically inside the container after a short delay.
    - **Environment Variables:** Do **not** update automatically; you must restart the Pod.
- **Best Practices:**
    - **Do not** store passwords or keys (use **Secrets** instead).
    - **Restart Deployments** (`kubectl rollout restart`) to ensure all changes take effect reliably
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
    name: mosquitto-config-file
data:
    mosquitto.conf: |
        log_dest stdout
        log_type all
        log_timestamp true
        listener 9001
```

#### 3 App config
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mosquitto
  labels:
    app: mosquitto
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mosquitto
  template:
    metadata:
      labels:
        app: mosquitto
    spec:
        containers:
          - name: mosquitto
            image: eclipse-mosquitto:2.0
            ports:
              - containerPort: 1883
            volumeMounts: 
              - name: mosquitto-config
                mountPath: /mosquitto/config
              - name: mosquitto-secret
                mountPath: /mosquitto/secret
                readOnly: true
        volumes:
          - name: mosquitto-config
            configMap:
              name: mosquitto-config-file
          - name: mosquitto-secret
            secret:
              secretName: mosquitto-secret-file
```
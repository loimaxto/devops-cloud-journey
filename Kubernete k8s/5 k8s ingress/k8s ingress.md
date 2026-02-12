### Command 

```bash
# add ingress-controller plugins
minikube addons enable ingress
minikube dashboard # setup dashboard for local environment

kubectl get ns
kubectl get all -n kubernetes-doashboard
kubectl apply -f ingress-dashboard.yaml
kubectl get all -n kubernetes-dashboard
kubectl get ingress -n kubernetes-dashboard
kubectl describe ingress dashboard-ingress -n kubernetes-dashboard

minikube tunnel 
```
### Why do this project?
Expose k8s doash to outside cluster, stimulate how to access application and setup load balance with secure method( https)

![](screenshots/Pasted%20image%2020260206133156.png)
### Instruction
1. install k8s ingress nginx( loadbalance) (play role as ingress-contronller)
2. create ingress file to public k8s dashboard
```txt
kubectl describe ingress dashboard-ingress -n kubernetes-dashboard
Name:             dashboard-ingress
Labels:           <none>
Namespace:        kubernetes-dashboard
Address:          192.168.49.2
Ingress Class:    nginx
Default backend:  <default>
Rules:
  Host           Path  Backends
  ----           ----  --------
  dashboard.com  
                 /   kubernetes-dashboard:80 (10.244.0.51:9090)
Annotations:     <none>
Events:
  Type    Reason  Age                From                      Message
  ----    ------  ----               ----                      -------
  Normal  Sync    15m (x2 over 16m)  nginx-ingress-controller  Scheduled for sync
```

3. add host (/etc/hosts) to doash.com to dashboard ip
### Example config file



doashboard-ingress.yaml
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: dashboard-ingress
  namespace: kubernetes-dashboard
spec:
  defaultBackend:
	  service: kubernetes-dashboard
	  port:
		  number: 80
  rules:
  - host: dashboard.com
    http:
      paths:
        - path: /
          pathType: Prefix
          backend:
            service:
              name: kubernetes-dashboard
              port:
                number: 80
```

## Config rules

#### 1 Multiple subdomain

#### 2 Configuring TLS Certificate - https://
- Ingress secrete using the same namespace as 
Need to config
- service, secret that the app using

Ingress file
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: dashboard-ingress
  namespace: kubernetes-dashboard
spec:
  tls:
  - hosts:
    - myapp.com
    secretName: myapp-secret-tls
  defaultBackend:
	  service: kubernetes-dashboard
	  port:
		  number: 80
  rules:
```
secret file
```yaml
apiVersion: v1
kind: Secret
metadata:
	name: myapp-secret-tls
	namespace: kubernetes-dashboard
data:
	tls.crt: base64 encoded cert
	tls.key: base64 encoded key
type: kuberentes.io/tls
```


## Useful Links: 
- Project repo: https://gitlab.com/twn-devops-bootcamp/latest/10-kubernetes/ingress 
- List of Ingress Controllers you can choose from: https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/ 
- Ingress Controller Bare Metal: https://kubernetes.github.io/ingress-nginx/deploy/baremetal/
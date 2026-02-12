
### Outline
1. Introduce: [structure](1%20introduce/structure.md)
2. Namespace: [k8s](.md)
3. Service:[k8s Services](4%20k8s%20service/k8s%20Services.md)
4. Ingress: [k8s ingress](5%20k8s%20ingress/k8s%20ingress.md)
5. Volume: [Volume](6%20volume/Volume.md)
6. Configmap Secret
	1. [k8s-deploy-mongodb](2%20k8s-deploy-mongodb/k8s-deploy-mongodb.md) 
	2. [[]]
7. 



---
K8s
- RBAC : manage users/ role
	1. Admin : namespace, volume, cluster
	2. User: within a namespace, specific cluster, pod


---
### Security requirements
- Using specify image:tag ( not latest)
- Configure a liveness probe on each container (check health app in container)
- Readiness Probe (check health startup)
- Configure resources for container( ram,cpu,limit)
- Don't expose **NodePort** for service

#### Command

```bash
	kubectl get namespace
	kubectl create namespace myname
	kubectl apply -f myconfigmap.yaml --namespace=myname
	kubectl get configmap -n mynamespace
	kubectl config setcontext --current --namespace=myname
```

### Namespace type 
**Contain**
- default: 
- kube-node-lease: manage node
- kube-public: contain configmap
- kube-system: don't modify

### Use cases

1. Group services
![](usecase1.png)

2. Isolate team resource

![](usecase2.png)


### how to use

```yaml
apiVersion: v1
kind: ConfigMap
metatdata:
	name: test-configmap
	namespace: myname
```

### Tips
Using `kubectx` tool to mange namespace
https://github.com/ahmetb/kubectx


##### Kubectl commands

```
kubectl get nodes
kubectl get pod
kubectl get services
kubectl create deployment nginx-name --image=nginx
kubectl get deployment
kubectl get replicaset
kubectl edit deployment nginx-depl
kubectl get all | grep mongodb
```

##### Create minikube cluster

```
minikube start
kubectl get nodes
minikube status
kubectl version
```

##### Delete cluster and restart in debug mode
```
minikube delete
minikube start --alsologtostderr
minikube status
```

##### Debugging

```
kubectl logs "INSERT POD NAME"
kubectl exec -it "INSERT POD NAME" -- bin/bash
```

##### Create mongo deployment

```
kubectl create deployment mongo-depl --image=mongo
kubectl logs mongo-depl-"INSERT POD NAME"
kubectl describe pod mongo-depl-"INSERT POD NAME"
```

##### Delete deplyoment

```
kubectl delete deployment mongo-depl
kubectl delete deployment nginx-depl
```

##### Create or edit config file

```
vim nginx-deployment.yaml
kubectl apply -f nginx-deployment.yaml
kubectl get pod
kubectl get deployment
kubectl get pod -o wide
```

##### Mange service
```bash
kubectl apply -f app-service.yaml
kubectl describe service nginx-service
kubectl get deployment app-deployment -o yaml > result.yaml # get status from /etcd

```

##### Delete with config

```
kubectl delete -f nginx-deployment.yaml
```

##### Metrics

```
kubectl top
``` 

The kubectl top command returns current CPU and memory usage for a cluster’s pods or nodes, or for a particular pod or node if specified.

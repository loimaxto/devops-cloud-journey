#### 4 connect cluster
```bash
kubectl config get-contexts
kubectl cluster-info
kubectl config use-context name

export KUBECONFIG=~/Desktop/mycluster-config.yaml
kubectl get nodes
```

**apply**
```bash
kubectl apply -f <name-file> -n <name-space>
```
## Summary
- Package management for k8s
- Create template
- Support plugins 
## 1 Instruction
1. create cluster digital ocean
	1. get login cluster credential
	2. chmod 400 login-k8s.yaml
	3. export KUBECONFIG=login-k8s

#### command
```bash
#connect to cluster
export KUBECONFIG=~/Desktop/mycluster-config.yaml
kubectl get nodes

# setup plugins server for helm
helm repo add bitnami https://charts.bitnami.com/bitnami  
helm search repo bitnami  
helm install my-release bitnami/<chart> # connect: cluster -> kubectl -> helm install

```
### Setup
```bash
helm
sudo apt-get install curl gpg apt-transport-https --yes
curl -fsSL https://packages.buildkite.com/helm-linux/helm-debian/gpgkey | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/helm.gpg] https://packages.buildkite.com/helm-linux/helm-debian/any/ any main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
```
### 1 Structure
- mychart
	- Chart.yaml: meta data
	- values.yaml: value for template files
	- charts/folder : chart dependencies
	- templates/folder
## 2 Deploy statefulset mongodb
deploy method:
- Config by myself
- Using template on helm chart
### command
```bash
# helm install <name> --values <file value> <chartname>
helm install mongodb --values helm-mongodb.yaml bitnami/mongodb
kubectl get secret # list secret
kubectl get secret mongodb -o yaml

helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm install nginx-ingress ingress-nginx/ingress-nginx --set controller.publishService.enabled=true

kubectl scale --replicas=0 --statefulset/mongodb

```

### Instruction
2 ways to create secret
- Create directly from cli (shoudn't cause might be leaked through log)
- Apply docker-config.json

1. Create secret component using cli
2. Configure deployment, pod

command
```bash
cat ~/.docker/config.json # registry credential

minikube start
minikube ssh

minikube cp minikube:/home/docker/.docker/config.json /home/lawrence/.docker/config.json

```

**Create secret**
Input
```shell
kubectl create secret docker-registry secret-tiger-docker \
  --docker-email=tiger@acme.example \
  --docker-username=tiger \
  --docker-password=pass1234 \
  --docker-server=my-registry.example:5000
```
config.json
```bash
kubectl create secret generic my-registry-key \
--from-file=.dockerconfig.json=.docker/config.json \
--type=kubernetes.io/dockerconfigjson

```

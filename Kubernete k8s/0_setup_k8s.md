## kubeadm

## k3s

link: https://docs.k3s.io/installation/requirements?os=debian

```bash
curl -sfL https://get.k3s.io | sh - # master

curl -sfL https://get.k3s.io | K3S_URL=https://myserver:6443 K3S_TOKEN=mynodetoken sh - # agent
curl -sfL https://get.k3s.io | K3S_URL=https://192.168.1.101:6443 K3S_TOKEN=mynodetoken sh -
```
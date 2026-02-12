## Overview
- Setup teleport for monitoring
	- setup nginx
	- install app
- NAT port to internet( using dynamic public ip cloudflare dns)


```bash
curl -4 ifconfig.me; echo  # show current public ip
```

#### 1 Forward port router
- **VNPT**: advanced -> advanced NAT


setup teleport server
```bash
wget https://get.gravitational.com/teleport-v13.2.0-linux-amd64-bin.tar.gz
tar -xzf teleport-v13.2.0-linux-amd64-bin.tar.gz
mv teleport/tctl /usr/local/bin/
mv teleport/tsh /usr/local/bin/
mv teleport/teleport /usr/local/bin/
teleport version && tctl version && tsh version
mkdir -p /etc/teleport
```

```bash
# setup tools to create certificate
apt install apache2-utils certbot python3-certbot-nginx -y 
	# create certificate
sudo certbot certonly --standalone -d teleport.quanghomelabdevops.dpdns.org --preferred-challenges http --agree-tos -m $EMAIL --keep-until-expiring

```
port note
- 80 nginx
- 443 https
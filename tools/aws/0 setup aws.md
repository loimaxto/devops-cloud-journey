
Setup VPC
1. Create vpc
2. subnet
3. internet gateway: expose vp to internet
4. route table: direct network traffic
5. security group: control inbound + outbound
6. vpc flow log : monitoring security, network

Create EC2
1. setup ssh
2. NAT gateway:  allow private subnet connects internet 
3. Reachability analyzer: 
4. Login public -> access private subnet

## SSH 
```bash
chmod 400 .ssh/docker-server.pem
ssh -i ~/.ssh/docker-server.pem ec2-user@ip
```
## Remove Resource
1. EC2
2. NAT gtw & Elastic IP address(Elastic IP)
3. EC2 connection endpoint
	1. VPC - Private link and Lattice - Endpoint( connect to other services)
	2. VPC - VPN  site to site connection
	3. VPC - VPN - private gtw
	4. VPC - VPN - customer gtw
4. delete VPC
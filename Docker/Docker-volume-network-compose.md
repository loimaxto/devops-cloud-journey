# Docker volume

## Volume type

### 1 Host volume
`docker run -v /host/dir/mount/data:/var/lib/mysql/data `

### 2 Anonymous volume

For each container, a folder is generated that gets mounted, docker automatically mount
`docker run -v /var/lib/mysql/data`

### 3 Named volume
Can reference the volume by name
Should be used in production
`docker run -v name:/container/data/path`

## Set volume in docker compose

```yaml
version: '3'
services:
    image: mongo
    ports:
     - 27017:27017
    environment:
     - MONGO_INITDB_ROOT_USERNAME=admin
     - MONGO_INITDB_ROOT_PASSWORD=password
    volumes:
     # mapping volume and container data folder
     - mongo-data:/data/db
  mongo-express:
    image: mongo-express
    restart: always
    ports:
     - 8081:8081
    environment:
     - ME_CONFIG_MONGODB_ADMINUSERNAME=admin
     - ME_CONFIG_MONGODB_ADMINPASSWORD=password
     - ME_CONFIG_MONGODB_SERVER=mongodb
    depends_on:
     - "mongodb"
volumes:
  # create physical hard drive to store
  mongo-data:
    driver: local
```
---

## Volume location
Each OS has different location
GG by yourself :))
# Docker compose
( similar to Docker swan)
docker-compose up -d

```yml
version: "3.8"
services:
	db1:
		image: mariadb:10.6
		volumes:
				- /db/mariadb-1:/var/lib/mysql
		environment:
				MYSQL_ROOT_PASSWORD: root
		ports:
				- "3307:3306"
		container_name: mariadb-1
		restart: always // restart computer will automically turn on
	app-backend:
		image: shoeshop:v3
		ports:
			- "8081:8080"
		restart: always
```

# Command
```bash
docker-compose up -d
docker exec -it <container-name> sh
docker inspect shoeshop-1   // inspect network of container
	gateway: gateway of container
	ipdress:

```

---
This command will build project without need to install java
* run : run container 
* --rm : after run remove temporary container
* -v 
```bash
docker run --rm -v `pwd`:/app --workdir="/app" maven:3.5.3-jdk-8-alpine mvn install -DskipTests=true

```

```bash
docker run -v /db/mariadb-1/:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root -p 3307:3306 --name mariadb-1 -d mariadb:10.6


mysql -h 192.168.192.99 -P 3307 -u root -p
mysql -h localhost -P 3307 -u root -p
```

In this lab, I will create 3 container
1. Nodejs 
2. Mongo
3. Mongo Express

## Setup
### Mongodb

```bash
docker network create < network- name >
# search on image document to run
docker run -d \
-p 27017:27017 \
-e MONGO_INITDB_ROOT_USERNAME=admin \
-e MONGO_INITDB_ROOT_PASSWORD=1234 \
--name mongodb \
--net mongo-network \
mongo:6.0-jammy

docker log

```


### Mongo express
```bash
docker run -d \
  -p 8081:8081 \
  --name mongo-express \
  --net mongo-network \
  -e ME_CONFIG_MONGODB_URL="mongodb://admin:1234@mongodb:27017/" \
  mongo-express:1.0.2-20-alpine3.19

```

### Docker compose
docker compose run command : ` docker compose -f file.name up `
docker compose **stop** command : ` docker compose -f file.name down `
```yaml
version: '3'
services:
  # my-app:
    # image: ${docker-registry}/my-app:1.0
    # ports:
     # - 3000:3000
  mongodb:
    image: mongo
    ports:
     - 27017:27017
    environment:
     - MONGO_INITDB_ROOT_USERNAME=admin
     - MONGO_INITDB_ROOT_PASSWORD=password
    volumes:
     - mongo-data:/data/db
  mongo-express:
    image: mongo-express
    # auto restart container ( unless-stoppped)
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
  mongo-data:
    driver: local
```
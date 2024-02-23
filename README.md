# USING DOCKER

- [USING DOCKER](#usingdoker)
  - [Image](#image)
  - [Container](#container)
    - [Mount](#mount)
  - [Volume](#volume)
    - [Backup](#backup)
    - [Restore](#restore)
  - [Network](#network)

## Image
list -> `docker image ls`

download image -> `docker image pull {image}:{tag}`

remove -> `docker image rm {image}:{tag}`

## Container

list -> `docker container ls`
- -a -> for all container

create -> `docker container create {param} {image}:{tag}`
- --name {containername}
- --publish {hostport}:{destinationport}
- --memory {b/k/m/g}
- --cpus {core}
- --env {key}={value}
- --mount {type="", source="", destination=""}
- --rm
- --network {networkname}


start -> `docker container start {containername}`

stop -> `docker container stop {containername}`

remove -> `docker container rm {containername}`

logs -> `docker container logs {containername}`

-  -f -> waiting new log

execute -> `docker container exec -it {containername} /bin/bash`

stats -> `docker container stats`

purne -> `docker container purne`

### Mount

| Param | Desc |
| ------ | ------ |
| Type | volume/bind |
| Source | Destination Host |
| Destination | Destination Container |
| Readonly | Readonly folder |

volume example -> 
```
--mount "type=bind, source=C:/Users/Azis/Documents/web-learning/docker/{dir},destination=/{dir}, destination="data/db"
```
bind example -> 
```
--mount "type=volume, source={volumename},destination=/data"
```

## Volume
list -> `docker volume list`

create -> `docker volume create {name}`

remove -> `docker volume rm {volumename}`

mount -> `--mount type=volume, source={volumename}, destination=data/db`

### Backup
`tar cvf backup/backup.tar.gz /data`

simple example -> 
``` powershell
docker container run --rm --name ubuntubackup --mount type=bind,source=C:\Users\Azis\Documents\web-learning\docker\backup,destination=/backup --mount type=volume,source=mongodata,destination=/data ubuntu:latest tar cvf /backup/backup.tar.gz /data
```

### Restore
`bash -c "cd /data && tar xvf /backup/backup.tar.gz --strip 1`

simple example -> 
```
docker container run --rm --name ubunturestore --mount type=bind,source=C:\Users\Azis\Documents\web-learning\docker\backup,destination=/backup --mount type=volume,source=mongorestore,destination=/data ubuntu:latest bash -c "cd /data && tar xvf /backup/backup.tar.gz --strip 1"
```

bind example -> 
```
--mount "type=bind, source=C:/Users/Azis/Documents/web-learning/docker/{dir},destination=/{dir}, destination="data/db"
```

## Network

list -> `docker network ls`

create -> `docker network create --driver {drivername} {networkname}`
- --driver {dafault=>bridge/host/none}

remove -> `docker network rm {networkname}`

connect -> `docker network connect {networkname} {containername}`

disconnect -> `docker network disconnect {networkname} {containername}`

```
# Use root/example as user/password credentials
version: '3.1'

services:

  mongo:
    image: mongo
    restart: always
    environment:
      MONGO_INITDB_ROOT_USERNAME: root
      MONGO_INITDB_ROOT_PASSWORD: example

  mongo-express:
    image: mongo-express
    restart: always
    ports:
      - 8081:8081
    environment:
      ME_CONFIG_MONGODB_ADMINUSERNAME: root
      ME_CONFIG_MONGODB_ADMINPASSWORD: example
      ME_CONFIG_MONGODB_URL: mongodb://root:example@mongo:27017/
```

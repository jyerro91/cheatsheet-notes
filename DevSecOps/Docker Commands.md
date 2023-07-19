# Docker Commands

## Containers
---
#### Check for running containers
```bash
docker ps
```

#### Check for existing containers, whether running or not
```bash
docker ps -a
```

#### Inspecting Container Metadata
```bash
docker inspect container_name_or_id | jq 
```
> jq is an additional program needs to be installed
> sudo apt install jq | `in ubuntu`

#### Removing Container
```bash
docker rm container_name_or_id
```
> remove all containers with this trick
```bash
docker rm $(docker ps -aq)
```

#### Remove the container when done running
> via the --rm flag
```bash
docker run --rm ubuntu:16.04 ls -lah
```

#### Start a container
```bash
docker start container_name_or_id
```
> Starting and interacting with the container
```bash
docker start -i container_name_or_id
```


## Images
---
#### View or List local images
```bash
docker images
```
#### Remove a specific image by ID or Name
```bash
docker rmi image_id_or_name:tag_combo
```
> remove all images
```bash
docker rmi $(docker images -q)
```

#### Running image
> Since that's not on my machine locally, it gets downloaded from Docker Hub (specifically https://hub.docker.com/_/ubuntu/).
> 
* Lists out the current working directory
* inside the container
```bash
docker run ubuntu:16.04 ls -lah
```
> This container runs the `ls -lah` container and then stops. Since the ls command is short-running (runs, then exits), the container will stop once the ls command finishes. Containers will only run as long as there's a process to run/monitor.

## Network
---

#### See available commands for network
```bash
docker network -h
```

#### Create new network
```bash
docker network create --driver=bridge sd-net
```
Docker has 2 main network types out of the box, however network drivers are based on a plugin system, so 3rd party ones can be used.

`bridge` - the default network type, used for single-host setups (all containers are on a single server)
`overlay` - A mesh network that can span multiple hosts (servers) - good for Docker Swarm

For our example here, we just create a default `bridge` network named `sd-net`. We can spin up some new containers just like before, except this time we'll add them into a network instead of link them.

```bash
# Redis
docker run -d --name=redis --network=sd-net redis:alpine

# MySQL
docker run -d --name=mysql \
    --network=sd-net \
    -e MYSQL_ROOT_PASSWORD=root \
    -e MYSQL_DATABASE=my-app \
    -e MYSQL_USER=app-user \
    -e MYSQL_PASSWORD=app-pass \
    mysql:5.7

# PHP
docker run -d --name=php \
    --network=sd-net \
    -v $(pwd)/application:/var/www/html \
    shippingdocker/php:0.1.0

# Nginx
docker run -d  --name=nginx \
    --network=sd-net \
    -p 80:80 \
    -v $(pwd)/application:/var/www/html \
    shippingdocker/nginx:0.2.0
```
That will spin up all 4 containers. They'll be in a network and can communicate with eachother using the same hostnames we used before, since we created the link "aliases" to match the container names.

> Note the distinction there, however. With --link, we can create the "hostnames", while with the --network option, a container's hostname is based on the container name (or container ID).


## Other

#### Commit

Creating new image from a modified container

```bash
# tagging uses semver
docker commit -a "Chris Fidao" -m "Installed nginx" container_name_or_id shippingdocker/nginx:0.1.0
```
`container_name_or_id` refers to the latest container created after you performed additional modification

#### Diff

```bash
# See latest changes in the container
docker diff container_name_or_id
```

#### History
```bash
docker history shippingdocker/nginx:0.1.0
docker history shippingdocker/nginx:0.2.0
```
This will not show every command we ran inside of the container, but it will show the commands we used to spin up the container and any commit message made when we commited changes to create the image we're seeing the history of.

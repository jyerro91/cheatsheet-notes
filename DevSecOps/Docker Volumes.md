# Docker Volumes
---

#### Share current directory with the container
```bash
docker run -it -p 80:80 -v ~/path/to/folder:/var/www/html ubuntu:16.04 bash
```

```bash
docker volume -h

docker volume ls
```

We'll see two volumes shared between the containers we spun up in the last video and my host machine. The MySQL container and the Redis container Dockerfile's both contain a VOLUME entry, which tells Docker to create a named volumed when a container is spun up from those images.

So, the volumes are created and shared to the host machine.

## Inspecting a Volume

We can see that by inspecting a volume and attempting to locate the files on our host machine.

```bash
docker volume inspect volume_name_or_generated_hash
```

We'll see a Mountpoint of something like `/var/lib/docker/volumes/volume_name_or_generated_hash/_data`.

On our Macintosh or Windows machines (but not on our Linux machines which can run Docker natively!) we run into an issue here. Those directories on our host machine do not exist. They only exist within the virtualization layer that is running Docker!

But we can still inspect them using a little trick - spinning up an Alpine linux container and sharing the root of our Macintosh's drive. We'll see the Alpine linux container is able to access the "hidden" Docker files, which contain the named volumes.

```bash
docker run --rm -it -v /:/vm-root \
    alpine:latest ls -lah /vm-root/var/lib/docker/volumes/volume_name_or_generated_hash/_data
```

## Creating and Using a Named Volue

We kill our currently running containers and volumes, and then create a new named volume.

```bash
docker stop $(docker ps -aq)
docker rm $(docker ps -aq)

docker volume rm $(docker volume ls -q)

# Create a new volume, giving it a name
docker volume create --driver=local --name=mysqldata

# See that it exists (and is empty at this point)
docker run --rm -it -v /:/vm-root \
    alpine:latest ls -lah /vm-root/var/lib/docker/volumes/mysqldata/_data
```

Let's re-create a MySQL containers, and we'll use this volume for the MySQL container:

```bash
# MySQL
docker run -d --name=mysql \
    -v mysqldata:/var/lib/mysql \
    mysql:5.7
```
If we `exec` into this container, we'll see that our old database (`my-app` and `whatever-test` still is present, and our root user still has the same password. The MySQL container is re-using the data that exists in the `mysqldata` named volume!

```bash
docker exec -it mysql bash

> mysql -u root -p
> show databases;
```
# Docker


## Create dockerfile

```dockerfile
FROM ubuntu:latest

# install lib dependencies
# debconf: delaying package configuration, since apt-utils is not installed
RUN apt-get update && apt-get install -y --no-install-recommends apt-utils

CMD ["echo", "Hello World!!"]
```

## How to make an image from a dockerfile

Run ```$ docker build -t image_name:tag_name ./```command to create an image from dockerfile. 
Here's an example.
> We can just skip adding ":tag_name", then it will be set to ":latest" by default. 
```bash
 $ cd /move/to/dockerfile/path
 $ docker build -t image_name:tag_name ./
```

```bash
 $ docker build -t image_name ./
```

We can simply create a container from docker run command with the image that we just created like below.
> We can also skip the ```tag name``` here, and then this command will create the container from ```image_name:latest```.
```bash
 $ docker run -t image_name:0.0 ./
```

## How to attach to terminal inside of containers
```bash
 $ docker exec -ti container_name_or_id bash
 $ docker exec -ti -u root container_name_or_id bash // as root
```

## How to set password for containers
 first, attach to a container as a root
```bash
 $ docker exec -ti -u root container_name_or_id bash // as root
```
 and then, run command below and set the password
```bash
 $ passwd               // set root password
 $ passwd [username]    // set user password
```


## Docker Commands

Delete all containers using the following command:
```bash
 $ docker rm -f $(docker ps -a -q)
```

Delete all stoped containers:
```bash
 $ docker container prune
```

Delete all dangling images:
```bash
 $ docker image prune
```

Delete all docker resources including images, containers, volumes, etc.. 
```bash
$ docker system prune
```

Delete docker images by their ID. 
```bash
$ docker rmi 
```

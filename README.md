# Docker

## Create dockerfile

```dockerfile
FROM ubuntu:latest

RUN apt-get update -y
RUN apt-get install htop -y

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

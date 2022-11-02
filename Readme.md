# A Hands-On Introduction To Docker Containers

Play with Docker: https://labs.play-with-docker.com/

- Basic Docker Info

```bash
# Return the version of the client and the server
docker version

# More details about the confivguration of Docker
docker info

# Show command we can use in Docker
docker
```

- Images and Containers

```bash
# Run Nginx Web Server
docker container run --publish 80:80 nginx

# run Nginx in a diferent port
docker container run --publish 81:80 nginx

# List of containers
docker container ls
```

- docker run vs docker start

```bash
# Run a container in background
docker container run --publish 80:80 nginx

# ls, just show running containers.
docker container ls

# run vs start
docker container ls -a

# start one of these containers
docker start <id-stopped container>

# Note: "docker container run" will start a new container
# "docker container start" will use a stopped container to start it.
```

- Create customized containers

```bash
# Container named GDGContainer
docker container run --publish 80:80 --detach --name GDGContainer nginx

# We can specify the port, the name, also the version of the image
# "-T" command tries to check Nginx configuration
docker container run --publish 8080:80 --name nginxtest -d nginx:1.23 -T
```

- docker logs for containers

```bash
docker logs --help
docker logs -f <container_id>
docker logs -n <container_id>
```

- Removing Containers

```bash
# Use "rm" from remove to delete a container
docker container rm  <container_id>

# Force a Delete
docker container rm  -f <container_id>
```

- Manage Multiple Containers: nginx, MySQl and httpd

```bash
# Run nginx container
docker container run --publish 80:80 --name nginx --detach nginx
# Testing Nginx
curl localhost:80


# Run mysql container, use random password
docker container run --publish 3306:3306 --detach --name mysql --env MYSQL_RANDOM_ROOT_PASSWORD=yes mysql
# Testing MySQL
# Check MySQL password
docker container logs <container-id> | grep PASSWORD
# Enter in MySQL container with bash
docker container exec -it <container-id> bash
# Conenct to MySQL Server
mysql -uroot -p<PASSWORD>

# Run httpd container
docker container run --publish 8080:80 --name httpd --detach httpd
# Testingh httpd
curl localhost:8080

# Deleting containers and images
docker container stop nginx, httpd, mysql
docker container rm nginx, httpd, mysql
docker container rmi nginx, httpd, mysql

```

- Using the shell in containers

```bash
# Running a container with
docker container run -it --name nginx nginx bash

# exit
exit

# Ubuntu contianer in a Mac
docker container run -it --nmae ubuntu ubuntu
# Installing packages
apt-get update
exit
# Alpine

# Running the container
docker container run -it alpine bash
# Enter in the shell of Alpine
docker container run -it alpine sh
# Installing packages
apk update
exit
```

- Dockerfiles: Creating our own images

```Dockerfile
FROM ubuntu
MAINTAINER edith edithpuclla20@gmail.com
RUN apt-get update
CMD ["echo", "Hello World from my first Docker Image"]
```

```bash
# Build Dockerfile
docker image build -t myimage:1.0.0 .
docker image ls
# run the container with our new image
docker container run myimage:1.0.0
```

- DockerHub: Pushing our image to DockerHub

```bash
docker login
docker image tag myimage:1.0.0 edithturn/myimage:1.0.0
docker image push edithturn/myimage:1.0.0
# Check your image in DockerHub: https://hub.docker.com/
```

- Dockerizing our web application (OPTIONAL)

```Dockerfile
# Use an official Node runtime as the parent image
FROM node:lts
# Set the working directory in the container to /app
WORKDIR /app
# Copy the current directory contents into the container at /app
ADD . /app
# Make the container's port 80 available to the outside world
EXPOSE 80
# Run app.js using node when the container launches
CMD ["node", "app.js"]
```

```bash
docker build -t node-app:0.1 .
docker run -p 4000:80 --name my-app node-app:0.1
```

- Volumes with Percona Monitoring and Mangement (OpenSource)

```bash
# Pull the image.
docker pull percona/pmm-server:2

# Create a volume
docker volume create pmm-data

# Run the image:
docker run --detach --restart always \
--publish 443:443 \
-v pmm-data:/srv \
--name pmm-server \
percona/pmm-server:2
```

Resources:
Some of these examples are taking from the amaizng course in Udemy of Bret Fisher and Inroduction to Docker in cloudskillsboost.

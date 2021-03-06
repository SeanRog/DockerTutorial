# Docker Cheat Sheet for Spring Developers

List all Docker Images
docker images -a
List All Running Docker Containers
docker ps
List All Docker Containers
docker ps -a
Start a Docker Container
docker start <container name>
Stop a Docker Container
docker stop <container name> -or- docker kill <container name>
Kill All Running Containers
docker kill $(docker ps -q)
View the logs of a Running Docker Container
docker logs <container name>
Delete All Stopped Docker Containers
Use -f option to nuke the running containers too.
Run A Docker Image
docker run <image name>
Run A Docker Image As A Background Process
docker run -d <image name>
Share Storage With Host
docker run -v <my host path>:<the container path> <image name>
Build A Docker Image
(From the directory of the Dockerfile) docker build -t <tag name> .
Port Mapping
docker run -p <host port>:<container port> <image name>
Remove A Stopped Docker Container
docker rm <container name>
Specify Environment Variable For Docker Container
docker run -e MY_VAR=my_prop <image name>

docker rm $(docker ps -a -q)
Remove a Docker Image From System
docker rmi <image name>
Show All Docker Images
docker images
Delete All Docker Images
docker rmi $(docker images -q)
Delete All Untagged (dangling) Docker Images
docker rmi $(docker images -q -f dangling=true)
Delete All Images
docker rmi $(docker images -q)
Remove Dangling Volumes
docker volume rm -f $(docker volume ls -f dangling=true -q)
SSH Into a Running Docker Container
Okay not technically SSH, but this will give you a bash shell in the container.

Interact with the docker container
sudo docker exec -it <container name> bash

Use Docker Compose to Build Containers
Run from directory of your docker-compose.yml file.

docker-compose build
Use Docker Compose to Start a Group of Containers
Use this command from directory of your docker-compose.yml file.

docker-compose up -d
This will tell Docker to fetch the latest version of the container from the repo, and not use the local cache.

docker-compose up -d – force-recreate
This can be problematic if you’re doing CI builds with Jenkins and pushing Docker images to another host, or using for CI testing. I was deploying a Spring Boot Web Application from Jekins, and found the docker container was not getting refreshed with the latest Spring Boot artifact.

#stop docker containers, and rebuild
docker-compose stop -t 1
docker-compose rm -f
docker-compose pull
docker-compose build
docker-compose up -d
Follow the Logs of Running Docker Containers With Docker Compose
docker-compose logs -f
Save a Running Docker Container as an Image

docker commit <image name> <name for image>
Follow the logs of one container running under Docker Compose

docker-compose logs pump <name>
Introduction to Docker CourseCheckout my Free Introduction to Docker Course!
Dockerfile Hints for Spring Boot Developers
Add Oracle Java to an Image
For CentOS/ RHEL

ENV JAVA_VERSION 8u31
ENV BUILD_VERSION b13
# Upgrading system
RUN yum -y upgrade
RUN yum -y install wget
# Downloading & Config Java 8
RUN wget – no-cookies – no-check-certificate – header "Cookie: oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/$JAVA_VERSION-$BUILD_VERSION/jdk-$JAVA_VERSION-linux-x64.rpm" -O /tmp/jdk-8-linux-x64.rpm
RUN yum -y install /tmp/jdk-8-linux-x64.rpm
RUN alternatives – install /usr/bin/java jar /usr/java/latest/bin/java 200000
RUN alternatives – install /usr/bin/javaws javaws /usr/java/latest/bin/javaws 200000
RUN alternatives – install /usr/bin/javac javac /usr/java/latest/bin/javac 200000
Add / Run a Spring Boot Executable Jar to a Docker Image
VOLUME /tmp
ADD /maven/myapp-0.0.1-SNAPSHOT.jar myapp.jar
RUN sh -c 'touch /myapp.jar'
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/myapp.jar"]

From centos tutorial

another way to keep docker container running
docker run -d <image name> tail -f /dev/null

install java on centos container
yum install java

login to docker repo
docker login --username=<yourusername> --email=<youremail>
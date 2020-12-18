# docker CLI & Dockerfile Cheat Sheet

## Table of Contents

### Introduction 
### 1 docker CLI 
#### 1.1 Container Related Commands 
#### 1.2 Image Related Commands 
#### 1.3 Network Related Commands 
#### 1.4 Registry Related Commands 
#### 1.5 Volume Related Commands 
#### 1.6 All Related Commands 
### 2. Dockerfile 
#### About the Authors 

## Introduction
Containers allow the packaging of your application (and everything that you need to run it) in a “container image”. Inside a container you can include a base operating system, libraries, files and folders, environment variables, volume mount-points, and your application binaries.

A “container image” is a template for the execution of a container — It means that you can have multiple containers running from the same image, all sharing the same behavior, which promotes the scaling and distribution of the application. These images can be stored in a remote registry to ease the distribution.

Once a container is created, the execution is managed by the container runtime. You can interact with the container runtime through the “docker” command. The three primary components of a container architecture (client, runtime, & registry) are diagrammed below:


#### 1. docker CLI
##### 1.1 Container Related Commands
###### Examples
All examples shown work in Red Hat Enterprise Linux

1. Run a container in interactive mode: #Run a bash shell inside an image
```pythin
$ docker run -it rhel7/rhel bash
```

Check the release inside a container 
```python
[root@.../]# cat /etc/redhat-release
```

2. Run a container in detached mode:
```python
$ docker run --name mywildfly -d -p 8080:8080 jboss/wildfly
```

3. Run a detached container in a previously created container network:
```python
$ docker network create mynetwork
$ docker run --name mywildfly-net -d --net mynetwork \
-p 8080:8080 jboss/wildfly
```

4. Run a detached container mounting a local folder inside the container:
```python
$ docker run --name mywildfly-volume -d \
-v myfolder/:/opt/jboss/wildfly/standalone/deployments/ \
-p 8080:8080 jboss/wildflyjboss/wildfly
```

5. Follow the logs of a specific container:
```python
$ docker logs -f mywildfly
$ docker logs -f [container-name|container-id]
```

6. List containers:
List only active containers
```python
$ docker ps
```
List all containers
```python
$ docker ps -a
```

7. Stop a container:
Stop a container
```python
$ docker stop [container-name|container-id]
```
Stop a container (timeout = 1 second)
```python
$ docker stop -t1
```
8. Remove a container:
Remove a stopped container
```python
$ docker rm [container-name|container-id]
```
Force stop and remove a container
```python
$ docker rm -f [container-name|container-id]
```
Remove all containers
```python
$ docker rm -f $(docker ps-aq)
```
Remove all stopped containers
```python
$ docker rm $(docker ps -q -f “status=exited”)
```

9. Execute a new process in an existing container:
Execute and access bash inside a WildFly container
```python
$ docker exec -it mywildfly bash
docker [CMD] [OPTS] [CONTAINER]
```

| Command  | Description |
| ------------- | ------------- |
| daemon  | Run the persistent process that manages containers  |
| attach  | Attach to a running container to view its ongoing output or to control it interactively  |



```python


commit
cp
create
dif f
exec
export
kill
logs
pause
port
ps
rename
restart
rm
run
start
stats
stop
top
unpause
update
wait
```
Run the persistent process that manages containers
Attach to a running container to view its ongoing output or to control it interactively
Create a new image from a container’s changes
Copy files/folders between a container and the local filesystem
Create a new container
Inspect changes on a container’s filesystem
Run a command in a running container
Export the contents of a container’s filesystem as a tar archive
Kill a running container using SIGKILL or a specified signal
Fetch the logs of a container
Pause all processes within a container
List port mappings, or look up the public-facing port that is NAT-ed to the PRIVATE_PORT
List containers
Rename a container
Restart a container
Remove one or more containers
Run a command in a new container
Start one or more containers
Display one or more containers’ resource usage statistics
Stop a container by sending SIGTERM then SIGKILL after a grace period
Display the running processes of a container
Unpause all processes within a container
Update configuration of one or more containers
Block until a container stops, then print its exit code

5. Tag an image:
Creates an image called “myimage” with the tag “v1” for the image jboss/wildfly:latest
```python
$ docker tag jboss/wildfly myimage:v1
```
Creates a new image with the latest tag
```python
$ docker tag <image-name> <new-image-name>
```
Creates a new image specifying the “new tag” from an existing image and tag
```python
$ docker tag <image-name>[:tag][username/] <new-image-name>.[:new-tag]
```

6. Exporting and importing an image to an external file:
Export the image to an external file
``python
$ docker save -o <filename>.tar
```
Import an image from an external file
```python
$ docker load -i <filename>.tar
```
7 Push an image to a registry:
```python
$ docker push [registry/][username/]<image-name>[:tag]
```
### 1.2 Image Related Commands
Examples
All examples shown work in Red Hat Enterprise Linux
1. Build an image using a Dockerfile:
Build an image
$ docker build -t [username/]<image-name>[:tag] <dockerfile-path>
Build an image called myimage using the Dockerfile in the same folder where the command was executed
$ docker build -t myimage:latest .
3: List the images:
$ docker images
4: Remove an image from the local registry:
$ docker rmi [username/]<image-name>[:tag]
2. Check the history of an image:
 Check the history of the jboss/wildfly image
$ docker history jboss/wildfly
Check the history of an image
$ docker history [username/]<image-name>[:tag]
docker [CMD] [OPTS] [IMAGE]
build
history
images
import
info
inspect
load
pull
push
rmi
save
search
tag
connect
create
disconnect
inspect
ls
rm
Build images from a Dockerfile
Show the history of an image
List images
Create an empty filesystem image and import the contents of the
tarball into it
Display system-wide information
Return low-level information on a container or image
Load an image from a tar archive or STDIN
Pull an image or a repository from the registry
Push an image or a repository to the registry
Remove one or more images
Save one or more images to a tar archive
(streamed to STDOUT by default)
Search one or more configured container registries for images
Tag an image into a repository
Connects a container to a network
Creates a new network with the specified name
Disconnects a container from a network
Displays detailed information on a network
Lists all the networks created by the user
Deletes one or more networks
1.3 Network related commands
docker network [CMD] [OPTS]
Command
Description
Command
Description
login
logout
create
inspect
ls
rm
events
inspect
Log in to a container registry server. If no server is specified then default is used
Log out from a container registry server. If no server is specified then default is used
Create a volume
Return low-level information on a volume
Lists volumes
Remove a volume
Get real time events from the server
Show version information
1.4 Network related commands
1.5 Volume related commands
1.6 Related commands
Default is https://index.docker.io/v1/
docker volume [CMD] [OPTS]
2. Dockerfile
The Dockerfile provides the instructions to build a container image through the
`docker build -t [username/]<image-name>[:tag] <dockerfile-path>`
command. It starts from a previously existing Base image (through the FROM clause) followed by any other needed Dockerfile instructions.
This process is very similar to a compilation of a source code into a binary output, but in this case the output of the Dockerfile will be a container image.
Example Dockerfile
This example creates a custom WildFly container with a custom administrative user. It also exposes the administrative port 9990 and binds the administrative interface publicly through the parameter ‘bmanagement’.
Use the existing WildFly image
FROM jboss/wildfly
Add an administrative user
RUN /opt/jboss/wildfly/bin/add-user.sh admin Admin#70365 --silent
  Expose the administrative port
EXPOSE 8080 9990
Bind the WildFly management to all IP addresses
CMD [“/opt/jboss/wildfly/bin/standalong.sh”, “-b”, “0.0.0.0”,
“-bmanagement”, “0.0.0.0”]
Command
Description
Command
Description
Command
Description
  
 Build the WildFly image
```python
  $ docker build -t mywildfly .
  ```
Run a WildFly server
```python
$ docker run -it -p 8080:8080 -p 9990:9990 mywildfly
  ```
Access the WildFly administrative console and log in with the credentials admin/Admin#70635
```python
open http://<docker-daemon-ip>:9990 in a browser

Using the example Dockerfile
FROM
MAINTAINER
RUN
CMD
LABEL
EXPOSE
ENV
ADD
COPY
ENTRYPOINT
VOLUME
USER
WORKDIR
ARG
ONBUILD
STOPSIGNAL
  ```
Sets the base image for subsequent
Sets the author field of the generated images
Execute commands in a new layer on top of the current image and commit the results
Allowed only once (if many then last one takes effect)
Adds metadata to an image
Informs container runtime that the container listens on the specified network ports at runtime
Sets an environment variable
Copy new files, directories, or remote file URLs from into the filesystem of the container
Copy new files or directories into the filesystem of the container
Allows you to configure a container that will run as an executable
Creates a mount point and marks it as holding externally mounted volumes from native host or other containers
Sets the username or UID to use when running the image
Sets the working directory for any RUN, CMD, ENTRYPOINT, COPY, and ADD commands
Defines a variable that users can pass at build-time to the builder using --build-arg
Adds an instruction to be executed later, when the image is used as the base for another build
Sets the system call signal that will be sent to the container to exit
Dockerfile instruction arguments
Command
Description
```python
$ mkdir -p www/
$ echo “Server is up” > www/index.html
$ docker run -d \
-p 8000:8000 \
--name=pythonweb \
-v `pwd`/www:/var/www/html \
-w /var/www/html \
rhel7/rhel \
/bin/python \
-m SimpleHTTPServer 8000
$ curl <container-daemon-ip>:8000
$ docker ps
$ docker inspect pythonweb | less
$ docker exec -it pythonweb bash
  ```
Example: Running a web server container
Create a directory (if it doesn’t already exist)
Make a text file to serve later
Run process in a container as a daemon
Map port 8000 in container to 8000 on host
Name the container “pythonweb”
Map container html to host www directory
Set working directory to /var/www/html
Choose the rhel7/rhel directory
Run the Python command for
a simple web server listening to port 8000
Check that the server is working
See that the container is running
Inspect the container
Open the running container and look inside


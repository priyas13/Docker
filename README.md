# Docker
Docker is used to run software packages called "containers". These containers are isolated from each-other and bundle their own application, tools, libraries and files. Containers can communicate with each other through well-defined channels. These containers are light-weight because they are run by single operating system. A container packages all the code and its dependencies so the application runs quickly and reliable from one computing environment to another. Docker helps us to run many containes simulatneously on a given host. Docker is written in go.
![docker-overview](https://github.com/priyas13/Docker/blob/master/docker-overview.png)
* A server is a type of long-running program called a daemon process.
* REST API resembles the interface that programs can use to talk to the daemon and instruct it what to do.
* Clients use the REST API to control and interact with the daemon through commands.


## Image
Image is a stand-alone, lightweight, executable package that includes everything needed to run a piece of software, including the code, a runtime, libraries, enviornment variables, and config files. We can create our own docker image or use existing image created by others. To build our own image, we create a Dockerfile with a simple syntax for defining the steps needed to create the image and run it.

## Container
Container is a runtime instance of an image. Containers are isolated from each other and its host machine. 

A simple example of docker is to run hello-world
```
docker run hello-world
```
![docker-hello-world](https://github.com/priyas13/Docker/blob/master/docker-hello-world.png)

As we could see in this example, Docker client first contacted the Docker daemon.
* Docker daemon is a service that runs on our host operating system. As we could see in the picture above, server is called the docker daemon. This docker daemon listens to Docker API requests and manages Docker objects like images, containers, networks etc. Docker clients or docker daemon can run on the same system or we can also connect a Docker client to a remote Docker daemon.
* Docker clients are the way that users interact with Docker. For example, command like docker run in the above example. The client sends this command to docker daemon, which carries them out. 
* Docker daemon pulled the "hello-world" image from the Docker Hub.
* A container is created which the instance of the image and which runs the executable that produces the output.

## Docker commands:
```
docker images
```
To see a list of all images on our system
```
docker run hello-world
```
Docker clients finds the image and loads up the container and then run the command in the container. It creates a container from docker images and actually run the actual application.
```
docker ps
```
Shows all the containers that are currently running
``` 
docker ps -a 
```
Shows all the containers that we ran. It has a STATUS column which shows that these containers exited a few minutes ago.

## Dockerfile
A Dockerfile is a simple text-file that contains the list of commands that the Docker client calls which creating an image.

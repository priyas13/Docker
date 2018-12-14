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
![dockerfile-image-container](https://github.com/priyas13/Docker/blob/master/docker-image-container.png)
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
```
docker --version 
```
Gives the current version of Docker installed
```
docker info or docker version 
```
Gives details about our docker
```
docker stop <container-id>
```
Stops the running container. I don't know why control+c was not working for me in mac. So I tried docker stop from another terminal window.
```
docker pull <name of existing image in docker hub>
```
It brings the image in our docker repository
```
docker run --name <name you want> <docker-image-name>
```
Launches the container (running instance of the <docker-image-name>) locally
 


## Dockerfile
A Dockerfile is a simple text-file that contains the list of commands that the Docker client calls which creating an image.

Let us look at the hello-world components:
hello.txt file looks like this
```
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (windows-amd64, nanoserver-1709)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run a Windows Server container with:
 PS C:\> docker run -it microsoft/windowsservercore powershell

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
 ```
 Dockerfile looks like this
 ```
 FROM scratch
 COPY hello /
 CMD ["/hello"]
 ```
Example:

Suppose I want to write few programs using Irmin platform and coding in OCaml. For this, we need to install Irmin and Ocaml in our local machine and also all its dependencies. But with the help of docker we can get portable Irmin runtime as an image, no installation is necessary. Then, our build can include the base Irmin image right alongside our ocaml code, ensuring that our code, its dependencies, and the runtime, all travel together. These portable images are present in a file called Dockerfile. 

Irmin dockerfile looks like this:

```
FROM alpine

EXPOSE 8443 8080
RUN set -ex ;\
    apk add --no-cache --purge -U \
       opam sudo make bash gcc musl-dev \
       gmp gmp-dev libressl-dev linux-headers m4 perl zlib-dev ;\
    adduser -D irmin ;\
    mkdir /data ;\
    chown irmin /data ;\
    export OPAMYES=1 ;\
    opam init ;\
    opam switch 4.06.1 ;\
    eval $(opam config env) ;\
    opam update ;\
    opam install irmin-unix ;\
    mv ~/.opam/4.06.1/bin/irmin / ;\
    apk del opam make bash gcc musl-dev gmp-dev libressl-dev \
       linux-headers m4 perl zlib-dev ;\
    rm -rf /var/cache/apk/* ;\
    rm -rf ~/.opam
USER irmin
WORKDIR /home/irmin
ENTRYPOINT [ "/irmin" ]
VOLUME /data
CMD [ "init", \
      "-a", "http://0.0.0.0:8080", \
      "-d", \
      "--verbosity=info", \
      "-s", "git", \
      "--root", "/data" \
    ]
```

Dockerfile defines what goes on in the environment inside our container. We need to may ports to the outside world and we should be specific what files we need to "copy in" to the environment. It is the list of steps needed to create the image. 

RUN : It includes any needed packages.

EXPOSE : Make port 8443 8080 available to the world outside this container.

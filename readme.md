Introduction to Linux Operating System and Basic Shell

Book Reference: https://tldp.org/LDP/intro-linux/html/

## Install Docker and Start a Linux Terminal

1. Install Docker from https://docs.docker.com/get-docker/

NOTE: You do not need to signup Docker account to use it.

2. Start a Ubuntu Linux Container from Docker

```
docker run -it ubuntu
apt-get update
apt-get install man
apt-get install file
```

## How to use Docker

Here are few basic commands to navigate with Dockers

* You need to keep Docker Engine daemon process running first. The easiest way is just keep the Docker Desktop app up running.
* Then you may use `docker` command in a terminal to control the Docker Engine
* Use `docker run --it --name=my-container <image_name>` to start a container. Note that it will start a new container per each command it runs! 

	It's recommended to use a explicit name if you want to reuse it.

	The container will be removed after you exit. If you do not want to automatically remove, use `--rm=false` option. Once a container is started, it will be saved, even if you exit. 

	The next time you may use use `docker start --name=my-container` to reuse it instead. Else you will have bunch of new containers listed under even after you exit under your Docker Desktop Containers list view.
* Use `docker ps` to list current running containers and their names. Use `--all` option to see all, including NOT running containers.
* To open another terminal to an existing running container, you would use `docker attach <container_name>`
	
	WARNING: When attaching to same user and terminal, one terminal STDOUT will affect the current attached terminal?
* To start an existing container, run `docker start --name=my-container`

	Note that start command will only start the container and will not connect to a shell terminal for you. If you want to start and attach at the same time, just add `-ia` options.
* Use `docker rm my-container` to delete the container

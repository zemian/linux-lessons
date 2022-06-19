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

	If you intend to use the container as long term interactive with multiple sesisons, Then we also recommend you to use `--rm=false` option. This will keep the container run running even if you exit the shell and not automatically remove it.

	After you exit (stopped the container), you may re-use the same container with `docker start` (see below) to restart it again. If you do do this, or just use another `docker run`, it will create another new container with different ID, and it will not reuse the same previous interactive container! You may see all containers with `docker ps --all`.
* Use `docker ps` to list current running containers and their names. Use `--all` option to see all, including NOT running containers (ones that has exited).
* To open another terminal to an existing running container, you would use `docker exec -it <container_name> bash`

	WARNING: Do not use `docker attach` to open a new terminal to existing container. The attach will create another terminal with same STDIN/OUT for all of your existing terminal instead, and it's usually not what you wanted.
* To start an existing container, run `docker start --name=<container_name>`

	WARNING: Do not use `docker start -ia` option to enter shell immediately. Because if you do exit, it will stop the container!. It's bettern to start the container separately, then use `docker exec` to start another terminal shell session.
* Use `docker rm my-container` to delete the container permanently.
* Use `docker exec --user nobody -it <container_name> bash` to start a container with another user other than default `root`.

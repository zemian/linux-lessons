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

	After you exit (stopped the container), you may re-use the same container with `docker start` (see below) to restart it again. If you do not do this, or just use another `docker run`, it will create another new container with different ID, and it will not reuse the same previous interactive container! You may see all containers with `docker ps --all`.
* Use `docker ps` to list current running containers and their names. Use `--all` option to see all, including NOT running containers (ones that has exited).
* To open another terminal to an existing running container, you would use `docker exec -it <container_name> bash`

	WARNING: Do not use `docker attach` to open a new terminal to existing container. The attach will create another terminal with same STDIN/OUT for all of your existing terminal instead, and it's usually not what you wanted.
* To start an existing container, run `docker start --name=<container_name>`

	WARNING: Do not use `docker start -ia` option to enter shell immediately. Because if you do exit, it will stop the container!. It's bettern to start the container separately, then use `docker exec` to start another terminal shell session.
* Use `docker rm my-container` to delete the container permanently.
* Use `docker exec --user nobody -it <container_name> bash` to start a container with another user other than default `root`.

## How to install packages in Ubuntu

```
apt-get update
apt-get install <package-name>
```

## How to add new user in Ubuntu

Use `adduser <user_name>` to add user, and `deluser <user_name>` to remove it.

Note: Do not use `useradd` command, as that is more lower level command that does not automatically setup home directory etc.

## How to mount writable disks in Docker

```
docker run -it -v /Users/zemian:/MacUsers/zemian --name myubuntu ubuntu
```

NOTE: That only folders listed under Docker Engine "Resources" > File Sharing are allow to be used above "bind mount" mapping!

We can also add volumns to existing container
https://stackoverflow.com/questions/28302178/how-can-i-add-a-volume-to-an-existing-docker-container

## How to export running server port to host

The following will map the container port 80 to the host port 8080

```
docker run -it -p 8080:80 -v /Users/zemian/myhtdocs:/usr/local/apache2/htdocs --name myhttpd httpd
```

## How to run MySQL db with Docker

```
docker run -it -p 4000:3306 --name mymysql -e MYSQL_ROOT_PASSWORD=test123 mysql
```

Now open another terminal to verify the server

```
docker exec -it mymysql bash
root> mysql -u root -p
```

NOTE: You can not connect to host port 4000 with root user. You need to grant remote host access first.

See more on https://hub.docker.com/_/mysql

IMPORTANT: You need to shutdown mysql from docker properly like this:

```
docker exec mysql /usr/bin/mysqladmin -uroot -ptest123 shutdown
```

## How to setup PHP/Apache/Mysql package in Docker

See https://www.sitepoint.com/docker-php-development-environment/

## How to run WordPress applicaiton with Docker

```
docker run -it --name mywordpress -p 3000:80 wordpress
```

NOTE: This docker will not provide a database store! You need to connect to a remote DB. You may use above example to setup MySQL with Docker, then try to connect it.

See https://hub.docker.com/_/wordpress

### Create a attachable network to bridge WordPress and MySQL

We can do this separately, or use `--network` option when creating container.

```
docker network create --attachable mynetwork
docker network connect mynetwork mymysql
docker network connect mynetwork mywordpress
```

## How to use docker-compose to setup WordPress and MySQL together

Create `docker-compose.yml` file:

```
services:
  wordpress:
    container_name: mydocker_mywordpress
    image: wordpress
    ports:
      - "3000:80"
    links:
      - mysql
    environment:
      WORDPRESS_DB_HOST: mysql
      WORDPRESS_DB_USER: root
      WORDPRESS_DB_PASSWORD: "test123"
      WORDPRESS_DB_NAME: wordpress
  mysql:
    container_name: mydocker_mymysql
    image: mysql
    volumes:
      - /Users/zemian/my-docker-volumes/mymysql:/var/lib/mysql
    environment:
      MYSQL_DATABASE: wordpress
      MYSQL_ROOT_PASSWORD: "test123"
```

Open an terminal window to where the file is saved and run:

```
docker-compose up
```

NOTE: The docker will destroy the containers when exit! So no data will be saved this way!

See https://medium.com/@habibridho/how-to-easily-setup-wordpress-using-docker-compose-102166c40cfa
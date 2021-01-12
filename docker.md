## docker Mastery: with Kubernetes +Swarm from a >docker Captain

>docker version

>docker info


---

## Container

>docker container run --publish 80:80 nginx

In the browser go to http://localhost/ and you will see nginx working on port 80.

 **run** command start new container

>docker container run --publish 80:80 --detach --name webhost nginx

**webhost** just a name for container

--detach tells docker to run in background.

>docker container run -d -p 3306:3306 --name db -e MYSQL_RANDOM_ROOT_PASSWORD=yes mysql

To  find generated password look at logs of container



---


 

>docker container ls

>docker container ls -a

 -a return all container also stoped one

>docker container stop 123


>docker container rm 123 321 132

>docker container rm -f 123

 **-f** used for force remove, if container still running, it stop it and then delete container.

>docker container logs -f webhost

**-f** display live logs, like watch in linux

>docker container inspect nginx

**inspect** show metadata about the container(config, volums, networking, etx)

>docker container stats

**stats** show live performance data for all containers

---

>docker container top webhost

list process that running inside container

>docker container run -it --name proxy nginx bash

**-i** keep session open to receive terminal input

**-t** simulate a real terminal like what SSH does

**bash** used to give terminal after container started (-it required) (use **exit** to get out of shell)

>docker container start -ai ubuntu

**-ai** it's similar to -it

**start** start used to start container that runned before and stoped. it's not create an new container

>docker container exec -it mysql bash

**exec** Run additional process in running container

>docker container run -it --rm --name ubuntu ubuntu bash

**--rm** used to remove container when exit out from shell.

---

## Network
All container in same network can talk to each other without specify -p 

>docker container port webhost

>docker network ls

>docker container run -d --name new-nginx --network my-app-net nginx

**connect** set container to new created network

>docker network connect NETWORK_ID CONTAINER_ID

>docker network disconnect NETWORK_ID CONTAINER_ID

>docker container exec -it my-nginx ping new-nginx 

Containers thats run inside new created network can talk to each other by there name

---

## Image

>docker image ls

>docker image rm IMAGE_ID

Images does not have name

>docker image history

**history** Show layers of changes made in image (not list of things that happened in the container)

> docker image pull nginx

> docker image tag nginx tohid1987/nginx

**tag** assign one or more tags to an image

> docker image push tohid1987/nginx

>docker image build -t custome-nginx .

Build an Dockerfile image that exist inside current directory

To specify a docker build referencing a file other tan default(Dockerfile) use **-f**

> docker image prune -a

which will remove all images you're not using

> docker system prune

will clean up everything

## Volume

> docker volume ls

>docker container run -d --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=True -v mysql-db:/var/lib/mysql mysql

We can set a name to volume. Volume will not be remove when container stoped or removed.

**we can set same volume to multiple container**

> documer volume create

## docker-compose

Docker compose automatically create new virtual networks for containers. (they can talk to each other using name)

> docker-compose up

> docker-compose up -d

setup volumes/networks and start all containers

> docker-compose down

stop all container and remove cont/vol/net

> docker-compose logs

> docker-compose down -rmi local

delete environment and local image as well, clean every thins all


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

> docker container run --name psql -d --health-cmd="pd_isready -U postgres || exit 1" postgres

This command used for health check of container, in **docker container ls** we will see container status like as healty or unhealty

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

In example swarm-stack-3 it combine docker-compose.yml with docker-compose.override.yml and then start it

> docker-compose -f docker-compose.yml -f docker-compose.test.yml up -d

First yml file is base compose file and second is the overrided one.

> docker-compose -f docker-compose.yml -f docker-compose.test.yml config > output.yml

This command used for production compose. it push two file into output.yml compose file.

> docker-compose down

stop all container and remove cont/vol/net

> docker-compose logs

> docker-compose down -rmi local

delete environment and local image as well, clean every thins all



## swarm

> docker info

""Swarm: inactive"" by default

> docker swarm init

> docker swarm init --advertise-addr IP_ADDRESS

activate swarm in docker

> docker swarm join --token SWMTKN-1-0yhgtwizgrxpzm9ypdx2k4okq6ebdki9gwbemfscxz3ncjv4kg-e1igyai5klb07nou8eao38tsy 192.168.65.3:2377

Used to add a worker to this swarm,

> docker node ls

> docker service create alpine ping 8.8.8.8

Service in a swarm replaces the docker run. 
Command ebove return an service Id.

> docker service ls

""replicas"" for one service will be 1/1 (running service/ specified for running)

> docker container ls

this command still work as before and it will list container running alpine as created above

> docker service ps SERVICE_ID

List the tasks of a service, also tasks that shotdown before.

> docker service update SERVICE_ID --replicas 3

It will increment container replica inside service to 3

> docker container rm -f CONTAINER_ID

It will remove new created container for service and swarm will recreate another one automatically

> docker service rm SERVICE_ID

Remove service and its containers as wll

https://play-with-docker.com/docker service 

> docker node update --role manager node2

Change node2 to be manager

> docker swarm join-token manager

Used to add manager to swarm. Command above will return command like:

docker swarm join --token SWMTKN-1-0yhgtwizgrxpzm9ypdx2k4okq6ebdki9gwbemfscxz3ncjv4kg-e1igyai5klb07nou8eao38tsy 192.168.65.3:2377

run this command on a node that you want to join it as manager to swarm.

## swarm service update

> docker service create -p 8088:80 --name web nginx:1.13.7

> docker service scale web=5

> docker service update --image nginx:1.13.6 web

It update image in all replicas one by one

> docker service update --publish-rm 8088 --publish-add 9090:80 web

> docker service update --force web

It will pick the least used nodes which is a form of rebalancing.(used when we add new node to remove one, or add service which huge one and need all node to be rebalancing.)

## swarm network

> docker network create --driver overlay **mydupal**

> docker service create --name psql --network **mydrupal** -e POSTGRES_PASSWORD=mypass postgres

> docker service create --name drupal --network **mydrupal** -p 80:80 drupal

Create an network and create two service using new created network with overlay driver.

## swarm stacks

Basically it's compose files for production of swarm. (services, networks and volumes)

> docker stack deploy -c example-voting-app-stack.yml votapp

Create all network and services inside yml file. If voteapp exist it will update it other wise create new one.

> docker stack ps votapp

list task running on app, show us the node that task runing on it

> docker stack services voteapp

list services inside voteapp, show us replica etc

## swarm secret

> docker secret create psql_user FILE_NAME

Create secret from file.

> echo "myDBpassWORD" | docker secret create psql_pass -

Create secret from text typed in command line.

> docker secret ls

list created secrets

> docker service create --name psql --secret psql_user --secret psql_pass -e POSTGRES_PASSWORD_FILE=/run/secrets/psql_pass -e POSTGRES_USER_FILE=/run/secrets/psql_user postgres

use created secret inside container
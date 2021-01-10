### docker Mastery: with Kubernetes +Swarm from a >docker Captain

>docker version

>docker info


---

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

---

### Network
All container in same network can talk to each other without specify -p 

>docker container port webhost

>docker network ls

>docker container run -d --name new-nginx --network my-app-net nginx

**network** set container to new created network

>docker network connect NETWORK_ID CONTAINER_ID

>docker network disconnect NETWORK_ID CONTAINER_ID

---



**bold**
***
Language
your code

---

1. Item 1
2. Item 2
3. Item 3
   * Sub item 1
   * Sub item 3
* Unordered item
* Unordered item
* Unordered item
---
|Name|Email|Address|    
|---|---|---|     
|John|john@example.com|Address1| 
|John|john@example.com|Address1| 
---
Your quote looks like this.
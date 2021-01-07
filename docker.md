### Docker Mastery: with Kubernetes +Swarm from a Docker Captain
```
docker version
docker info
```
---
```
docker container run --publish 80:80 nginx
```
>In the browser go to http://localhost/ and you will see nginx working on port 80.

> **run** command start new container
```
docker container run --publish 80:80 --detach --name webhost nginx
```
>--detach tells docker to run in background.

> **webhost** just a name for container
```
docker container ls
docker container ls -a
```
> -a return all container also stoped one
```
docker container stop 123
```
```
docker container rm 123 321 132
docker container rm -f 123
```
> **-f** used for force remove, if container still running, it stop it and then delete container.
```
docker container logs -f webhost
```
---
```
docker container top webhost
```
> list process that running inside container
---



**bold**
***
```Language
your code
```

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
>Your quote looks like this.
# How To Create Docker Image Using Dockerfile

Docker has two ways to create a container image

- Create a Container, run several commands, and then use docker commit to generate a new image
- Create a Dockerfile and then use docker build to create an image

This topic is covered in Docker section in [DevOps in Depth]. In case you are interested in knowing more about the course you can check our website.

In this repo, we are going to create container and image using Dockerfile and docker build command.

## Prerequisite

- Docker 1.13 and above
- Linux OS
- Basic Linux Knowledge

Create a Directory

```sh
mkdir  docker-build
```

cd to directory:

```sh
cd docker-build
```
Create index.html file

```sh

cat <<EOF> index.html
> <html>
> <head>
> <title>Welcome to my excellent blog</title></head>
> <body bgcolor=blue>
> <h1>Welcome to my blog</h1>
> <h2>Jeeviacademy</h2>
> </body>
> </html>
> EOF
```
Copy index.html file and create a Dockerfile 

```sh
cat <<EOF>Dockerfile
> FROM httpd:latest
> COPY index.html /usr/local/apache2/htdoc/index.html
> EOF
```
Display Dockerfile

```sh

[root@lb1 docker-build]# cat Dockerfile 
FROM httpd:latest
COPY index.html /usr/local/apache2/htdoc/index.html
[root@lb1 docker-build]# 

```
Build a Docker image using docker build command

docker build -t httpbuild .

```sh

[root@lb1 docker-build]# docker build -t httpbuild .
Sending build context to Docker daemon 3.072 kB
Step 1/2 : FROM httpd:latest
 ---> 6e794a483258
Step 2/2 : COPY index.html /usr/local/apache2/htdocs/index.html
 ---> 678c196812a6
Removing intermediate container 2942f4342e6a
Successfully built 678c196812a6

```

To see docker images

```sh
docker images
```

```sh
[root@lb1 docker-build]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
httpbuild           latest              678c196812a6        14 seconds ago      145 MB
httpbuildsrv        latest              5efcc7000438        6 minutes ago       145 MB
httpserver          v1                  a7759e0292a5        About an hour ago   145 MB
docker.io/httpd     latest              6e794a483258        10 days ago         145 MB
docker.io/alpine    latest              042a816809aa        2 weeks ago         7.05 MB
docker.io/busybox   latest              66ba00ad3de8        3 weeks ago         4.87 MB

```
create a container using docker run command

```sh

[root@lb1 docker-build]# docker run -d -p 8083:80 --name httpbuildct httpbuild
7e3a50e61bf7805b6cea96a580ac54d04a77c93f061d70d37e1d6c5183946bb4

```
To see what are the container running in the system

```sh
docker ps

```

```sh

[root@lb1 docker-build]# docker ps
CONTAINER ID        IMAGE               COMMAND              CREATED              STATUS              PORTS                  NAMES
31f5be485bdc        httpbuild           "httpd-foreground"   About a minute ago   Up About a minute   0.0.0.0:8083->80/tcp   httpbuildct
7e3a50e61bf7        httpbuildsrv        "httpd-foreground"   7 minutes ago        Up 7 minutes        0.0.0.0:8082->80/tcp   httpbuildsrv
67b3a315bd4b        httpserver:v1       "httpd-foreground"   About an hour ago    Up About an hour    0.0.0.0:8081->80/tcp   httpd-v1
21dd4b6e218b        httpd               "httpd-foreground"   About an hour ago    Up About an hour    0.0.0.0:8080->80/tcp   httpserver
[root@lb1 docker-build]# 

```

To hit the website URl
```sh
curl -vk localhost:8083
```

```sh
[root@lb1 docker-build]# curl -vk lb1.dev.com:8083
* About to connect() to lb1.dev.com port 8083 (#0)
*   Trying 127.0.1.1...
* Connected to lb1.dev.com (127.0.1.1) port 8083 (#0)
> GET / HTTP/1.1
> User-Agent: curl/7.29.0
> Host: lb1.dev.com:8083
> Accept: */*
> 
< HTTP/1.1 200 OK
< Date: Sat, 28 Jan 2023 06:03:30 GMT
< Server: Apache/2.4.55 (Unix)
< Last-Modified: Sat, 28 Jan 2023 05:54:07 GMT
< ETag: "97-5f34c9c2fddc0"
< Accept-Ranges: bytes
< Content-Length: 151
< Content-Type: text/html
< 
<html>
<head>
<title>Welcome to my excellent blog</title></head>
<body bgcolor=blue>
<h1>Welcome to my blog</h1>
<h2>Jeeviacademy</h2>
</body>
</html>
* Connection #0 to host lb1.dev.com left intact
```
## License

MIT

[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)
   [DevOps in Depth]: <https://www.jeeviacademy.com/devops-in-depth/>
   [Landrush]: <https://github.com/vagrant-landrush/landrush>
   [hostmanager]: <https://github.com/devopsgroup-io/vagrant-hostmanager>
   





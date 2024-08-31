### Docker

1.to look what are the images we have :

```sh
docker images
```

2.to look what are the container we have :

```sh
docker ps
```

3.to look use a image from dockerhub :

```sh
docker pull [name:versiton]
```

if we don't give any versiton it will give us latest versiton

4.to RUN a image :

```sh
docker run [image name:tag(versiton)]
```

5.to run a container in background and print container id, this will not block terminal :

```sh
docker run -d [image name:tag(versiton)]
```

6.If we give "-d" flag then we won't see log, for viewing log :

```sh
docker log [container id]
```

7.if we don't havea a image localy but it is available on docker hub then we can:

```sh
docker run [image name:tag(versiton)]
```

it will try to find locally and if can't find then pull that image and run it.

8.Build a docker file with a name and versiton and current directory "."

```sh
docker build -t my-app:1.0.0 .
```

## Explse the posrt

Application inside contaienr runs in an isolated Docker network, we need to expose the contaienr port to the computer.it called port binding

1.to stop container :

```sh
docker stop [container id]
```

1.to run on background and on a specific port :

```sh
docker run -d -p 9000:80 [image name:tag(versiton)]
```

9000 is host port and 80 is container port

Docker run command creates a new container it doesn't use previous one.

2.to see all contaienr wheater it is running or not :

```sh
docker ps -a
```

if we only run ps then it give the list of running container

3.Don't wannt create new container and run a previous container :

```sh
docker start [container id]
```

4.Memorize a container id is hard for this we can give a name by:

```sh
docker run --name web_app -p 9000:80 [image name:tag(versiton)]
```

Now we can work with "web_app"

5.Repository vs Registry
Repo:collection of related images with same name but diffrent versions and registry is collection of repositories.

6.Dockerfile: This is a text document that contains commands to assemble an image.docker can build an image by reading those instructions

### Structer of doc file

1.FROM:build this image from the specified image
2.RUN:will execute any command in a shell inside the container verionment
3.COPY:copies files or directories and adds them to the filesystem of the container. whle "RUN" is executed in the container, "COPY" is exectued on the host
ex:COPY src /app/
it will copy src directory into app directory

4. WORKDIR:sets the working directory for all following commands
   ex:WORKDIR /app
   for folling command it use this directory

5.CMD:the instruction that is to be executed when a docker container starts

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

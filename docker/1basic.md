### Image vs command

container is arunning environment for image.we can bind port for talk to application running inside of container.

1.To get the terminal of a running container and run some command

```sh
docker exe -it [contaienr id]
```

-it means it open in intrective way

2.Run mondodb and give environment varibale

```sh
docker run -p 27017:27017 -d
e MONGO_INITDB_ROOT=admin
e MONGO_INITDB_PASSWORD=123
```

3.to see the last activity of a container

```sh
docker logs [container id ] | tail
```

4.It will start all container mentioned in mongo.yaml file

```sh
docker-compose -f mongo.yaml up
```

5.It will stop all container mentioned in mongo.yaml file

```sh
docker-compose -f mongo.yaml down
```

5.delete a container

```sh
docker rm [container id]
```

5.delete a image

```sh
docker rmi [image id]
```

6.we can rum command on bash or sh

```sh
docker exec -it [container id] /bin/bash
```

```sh
docker exec -it [container id] /bin/sh
```

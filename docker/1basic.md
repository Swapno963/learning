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

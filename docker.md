## Docker Notes

####  To start a container:

```
docker container run -d --name nginx-test -p 8080:80 nginx
```

Note the `-d`, it starts it in the background.

#### Attach to container:

```
docker container attach nginx-test
```
However, this will stop the container at exit. Attach using this command, to quit from the attached process and not from the running container:

```
docker container attach --sig-proxy=false nginx-test
```

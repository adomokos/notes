## Docker Notes

####  To start a container

```
docker container run -d --name nginx-test -p 8080:80 nginx
```

Note the `-d`, it starts it in the background.

#### Attach to container

```
docker container attach nginx-test
```
However, this will stop the container at exit. Attach using this command, to quit from the attached process and not from the running container:

```
docker container attach --sig-proxy=false nginx-test
```
#### Exec in the container

The `exec` command is more interactive, it spawns a new process within the container that I can interact with:

```
docker container exec nginx-test cat /etc/debian_version
```
Fork a bash process, use the `-i` and `-t` flags to keep open console access to our container. The `-i` means `--interactive`, which instructs Docker to keep `stdin` open so that we can send command to the process. The `-t` is for `--tty`.
```
docker container exec -i -t nginx-test /bin/bash
```

#### Logs

The logs command allows you to interact with the `stdout` stream of your containers.

```
docker container logs --tail 5 nginx-test
```
In real time:
```
docker container logs -f nginx-test
```
Since time:
```
docker container logs --since 2017-06-24T15:00 nginx-test
```

To get the date in the container:
```
docker container exec nginx-test date
```

#### Top

The top command lists the processes running within the container:

```
docker container top nginx-test
```

#### Stats

The stats command provides real-time information on either the specified container or, if you don't pass a container name or ID, all running containers:

```
docker container stats nginx-test
```

#### Update Resource Limits

```
docker container update --cpu-shares 512 --memory 128M --memory-swap 256M nginx-test
```

#### Inspect

```
docker container inspect nginx-test
```

#### Pause/Unpause Continer

```
docker container pause nginx-test
```

#### Prune All Stopped Containers

```
docker container prune
```

#### To Run MS SQL Server in Docker

```
$: docker pull microsoft/mssql-server-linux:2017-latest

$: docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=<the-password>' \
   -p 1433:1433 --name sql1 \
   -d microsoft/mssql-server-linux:2017-latest
```

#### Docker Networking

To pull an image:

```
docker image pull redis:alpine
docker image pull russmckendrick/moby-counter
```

To create a network:
```
docker network create moby-counter
```
To launch the container in that network:
```
docker container run -d --name redis --network moby-counter redis:alpine
docker container run -d --name moby-counter --network moby-counter -p 8080:80 russmckendrick/moby-counter
```

Ping the redis host from the moby-counter:
```
docker container exec moby-counter ping -c 3 redis
```

Let's see what the /etc/hosts file has to resolve `redis` to an IP address:
```
docker container exec moby-counter cat /etc/hosts
```
If no sign of `redis` there, check `/etc/resolv.conf`:
```
docker container exec  moby-counter cat /etc/resolv.conf
```
To see how the name server is handling this:
```
docker container exec moby-counter nslookup redis 127.0.0.11
```


# Tag latest
This tag refers to the latest version of bird and debian available when I built images

# Tag 2.0.11
This image is based on the 2.0.11 version of BIRD: https://gitlab.nic.cz/labs/bird/-/tree/v2.0.11

# Tag 2.0.10
This image is based on the 2.0.10 version of BIRD: https://gitlab.nic.cz/labs/bird/-/tree/2.0.10

# Tag 2.0.9
This image is based on the 2.0.9 version of BIRD: https://gitlab.nic.cz/labs/bird/-/tree/v2.0.9

# Tag 2.0.8
This image is based on the 2.0.8 version of BIRD: https://gitlab.nic.cz/labs/bird/-/tree/v2.0.8

# Tag 2.0.7
This image is based on the 2.0.7 version of BIRD: https://gitlab.nic.cz/labs/bird/-/tree/v2.0.7

# Bird

## Build

You need to provide a configuration file named `bird.conf`, the best way is to provide the directory where you store your configuration.

```
docker build . -t acorso/bird
```

## Run

You need to provide a configuration file named `bird.conf`, the best way is to provide the directory where you store your configuration.

```
docker run -d -p 179:179 --name my_bird \
--privileged --restart=always \
acorso/bird
```

You can check the status
```
> $ docker exec -it my_bird birdc show status
BIRD 2.0.11 ready.
BIRD 2.0.11
Router ID is 172.17.0.2
Hostname is 0576a450a9da
Current server time is 2022-12-19 12:08:21.698
Last reboot on 2022-12-19 12:07:59.622
Last reconfiguration on 2022-12-19 12:07:59.622
Daemon is up and running
```


***Tips***
I often link my bird socket to the bird-exporter (prometheus), to do so, I attach a directory to share the socket.

```
docker run -d -p 179:179 --name my_bird \
--privileged --restart=always \
-v ./my_directory:/etc/bird:rw \
-v ./shared_socket:/var/run:rw \
acorso/bird
```


## Example

```
> $ docker run -d -p 179:179 --name my_bird \
--privileged --restart=always \
-v ~/project/bird/conf:/etc/bird:rw \
acorso/bird
8d42f923bb813671e683e22ae2d5bffc819dd023089d6ec370481a4d573e1f4b


> $ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
8d42f923bb81        acorso/bird         "bird -c /etc/bird/b…"   4 seconds ago       Up 3 seconds        0.0.0.0:179->179/tcp   my_bird


> $ docker logs my_bird
bird: Started


> $ docker exec -it my_bird birdc show protocol
BIRD 2.0.7 ready.
Name       Proto      Table      State  Since         Info
device1    Device     ---        up     10:02:30.910
peer1      BGP        ---        start  10:02:30.910  Connect
```


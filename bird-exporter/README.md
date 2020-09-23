# Tag latest
This tag refers to the latest version of bird-exporter


# Tag 1.2.5
This image is based on v1.2.5 of bird-exporter - https://github.com/czerwonk/bird_exporter/releases/tag/1.2.5


# Bird

## Build

```
docker build . acorso/bird-exporter
```

## Run
```
docker run -d -p 9324:9324 --name bird-exporter1 \
--restart=always \
-v ./bird_socket/bird.ctl:/var/run/bird.ctl:r \
acorso/bird-exporter
```

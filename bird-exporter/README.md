# Tag latest
This tag refers to the latest version of bird-exporter
For BIRD v2 (by default)

# Tag 1.4.2
This image is based on v1.4.2 of bird-exporter - https://github.com/czerwonk/bird_exporter/releases/tag/1.4.2

# Tag 1.2.5
This image is based on v1.2.5 of bird-exporter - https://github.com/czerwonk/bird_exporter/releases/tag/1.2.5


# Bird

## Build

```
docker build . -t acorso/bird-exporter
```

## Run
```
docker run -d -p 9324:9324 --name bird-exporter1 \
--restart=always \
-v ./shared_socket/bird.ctl:/var/run/bird.ctl:rw \
acorso/bird-exporter
```

## Example

```
> $ docker run -d -p 9324:9324 --name my_bird-exporter \
--restart=always \
-v /tmp/shared_socket/bird.ctl:/var/run/bird.ctl:rw \
acorso/bird-exporter

f44ffeb6451d358c3defe0709e04162c8b2bc9b827ef9738583bbfbef306f6c4


> $ docker logs bird-exporter1
time="2023-06-23T13:22:45Z" level=info msg="Starting bird exporter (Version: 1.4.2)"
time="2023-06-23T13:22:45Z" level=info msg="Listening for /metrics on :9324 (TLS: false)"

> $ curl localhost:9324/metrics

# HELP bird_protocol_changes_update_export_accept_count Number of outgoing updates being accepted
# TYPE bird_protocol_changes_update_export_accept_count gauge
bird_protocol_changes_update_export_accept_count{export_filter="ACCEPT",import_filter="ACCEPT",ip_version="4",name="kernel1",proto="Kernel"} 0
...
```



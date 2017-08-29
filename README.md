# docker-janus
`docker-janus` is a Debian 8 based docker image for [Meetecho's Janus Gateway](https://github.com/meetecho/janus-gateway)

Visiting `http://localhost:8088/janus/info` in your browser should provide you with the build info of janus in JSON format.

A full set of default janus config files are in `./janus` folder, which is referenced as a volume in the `docker-compose.yml` file for docker-compose to use. 

## build criteria
`janus-gateway` is built with the following configured options disabled, as I do not have the need for them to be enabled by default:
```
./configure --prefix=/opt/janus --enable-post-processing --disable-docs --disable-boringssl --disable-mqtt --disable-rabbitmq
```

## default build
There is a `Makefile`, with some directives on building janus. Have a look at that file and check the options. Issuing a `make` will run the default build with the options set below.

```
DataChannels support:      yes
BoringSSL (no OpenSSL):    no
Recordings post-processor: yes
TURN REST API client:      yes
Doxygen documentation:     no
Transports:
    REST (HTTP/HTTPS):     yes
    WebSockets:            yes (new API)
    RabbitMQ:              no
    MQTT:                  no
    Unix Sockets:          yes
Plugins:
    Echo Test:             yes
    Streaming:             yes
    Video Call:            yes
    SIP Gateway:           yes
    Audio Bridge:          yes
    Video Room:            yes
    Voice Mail:            yes
    Record&Play:           yes
    Text Room:             yes
```

## docker build `--build-arg`
`--build-arg` provides away to override some build runtime arguments. Have a look at the `Dockerfile` for the `ARG` arguments to override.

Example build with `rabbitmq`, `paho-mqtt`, `boringssl` enabled, and `data-channels` disabled:
```
root@mcroth:~/sandbox/docker-janus# docker build --build-arg JANUS_WITH_BORINGSSL=1 --build-arg JANUS_WITH_PAHOMQTT=1 --build-arg JANUS_WITH_RABBITMQ=1 --build-arg JANUS_WITH_DATACHANNELS=0 -t mcroth/docker-janus:latest .
```


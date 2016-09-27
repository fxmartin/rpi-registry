# rpi-registry

As per the docker images best practices, it is advised to only run images from known repositories. As I was not able to find a proper docker image for running docker distribution on ARM, I started a quite long and bumpy journey to build my own.

## How to use this image

TODO: Add explanation on creating self signed certificates.

Run it with the below command:

```
docker run -d -p 5000:5000 -v /home/pirate/registry_certs:/certs -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.cert -e REGISTRY_HTTP_TLS_KEY=/certs/domain.key --restart=always --name registry-srv -v /mnt/usb/registry:/var/lib/registry armbuild/registry
```

## How to build this image

It started with how to compile a GO application. That's the easy part as I can cross-compile on my Mac by running the below command:

```
docker run -e GOOS=linux -e GOARCH=arm -v /tmp/crosstest:/go/bin  golang go get github.com/docker/distribution/cmd/registry/...
```

After that it's only a matter of cloning the github image: ```test```, replacing the registry/registry by the one you just compile and build the image by entering the command:

```
docker build --tag fxmartin/rpi-registry .
```

The resulting image is small, only 22.28 Mb:

```
REPOSITORY                                          TAG                 IMAGE ID            CREATED             SIZE
fxmartin/rpi-registry                               latest              9fdc8b7c58d4        38 minutes ago      22.28 MB
swarm-master.local:5000/fxmartin/rpi-registry       local               9fdc8b7c58d4        38 minutes ago      22.28 MB
```

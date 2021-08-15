# ContainerHub
Catalogue of different tested container builds. \
Docker is used for the operations.

## Docker commands Cheatsheet (as of 2021)

### Images

* List all locally stored images.
```sh
docker [image] ls
```

* Create a container image.
```sh
docker create [IMAGE]
```

* Create an image from a Dockerfile
```sh
docker build [URL/FILE]
```

* Create an image from a Dockerfile with Tags
```sh
docker build -t <tag> [URL/FILE] # Use "." for $PWD
```

* Pull an image from a registry
```sh
docker pull [IMAGE]
```

* Push an image to a registry
```sh
docker push [IMAGE]
```

* Create an image from a container
```sh
docker commit [CONTAINER] [NEW_IMAGE_NAME]
```

* Show the history of an image
```sh
docker history [IMAGE]
```

* Remove an image
```sh
docker rmi [IMAGE]
```

* Save an image to a tar archive
```sh
docker save [IMAGE] > [TAR_FILE]
```

* Create an image from a tarball
```sh
docker import [URL/FILE]
```

* Load an image from a tar archive or stdin
```sh  
docker load [TAR_FILE/STDIN_FILE]
```

### Containers

* Run a new container.
```sh
docker run [IMAGE]
```

* Rename an existing container.
```sh
docker rename [CONTAINER_NAME] [NEW_CONTAINER_NAME]
```

* Start a container and keep it running in the background (detached).
```sh
docker run -d [IMAGE]
```

* Start a container and creates an interactive bash shell in the container.
```sh
docker run -it [IMAGE]
```

* Remove container after it exits.
```sh
docker run --rm [IMAGE]
```

* Execute command inside already running container.
```sh
docker exec -it [CONTAINER]
```

* Stop a running container through SIGTERM.
```sh
docker stop [CONTAINER]
```

* Stop a running container through SIGKILL.
```sh
docker kill [CONTAINER]
```

* Start a stopped container
```sh
docker start [CONTAINER]
```

* Delete a container (if it is not running).
```sh
docker rm [CONTAINER]
```

* Delete all running and stopped containers
```sh
docker container rm -f $(docker ps -aq)
```

* Update the configuration of the container.
```sh
docker update [CONTAINER]
```

### Network

* List networks
```sh
docker network ls
```

* Show information on one or more networks
```sh
docker network inspect [NETWORK]
```

* Connects a container to a network
```sh
docker network connect [NETWORK] [CONTAINER]
```

* Disconnect a container from a network
```sh
docker network disconnect [NETWORK] [CONTAINER]
```

* Remove one or more networks
```sh
docker network rm [NETWORK]
```

### Monitoring

* Lists both running and stopped containers
```sh
docker ps -a
```

* Show live resource statistics of container
```sh
docker stats [CONTAINER]
```

* Show port mapping for a container
```sh
docker port [CONTAINER]
```

* List real-time events from a container
```sh
docker events [CONTAINER]
```

* List the logs from a running container
```sh
docker logs [CONTAINER]
```

* Show running processes in a container
```sh
docker top [CONTAINER]
```

* Show changes  on the filesystem since creation
```sh
docker diff [CONTAINER]
```






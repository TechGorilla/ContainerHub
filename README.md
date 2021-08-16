# ContainerHub
Catalogue of different tested container builds. \
Docker is used for the operations.

## Docker commands Cheatsheet
- [](#)
  - [](#)
    - [Images](#images)
    - [Containers](#containers)
    - [Network](#network)
    - [Monitoring](#monitoring)


### Images

```sh
# List all locally stored images.
docker [image] ls
# Create a container image.
docker create [IMAGE]
# Create an image from a Dockerfile
docker build [URL/FILE]
# Create an image from a Dockerfile with Tags
docker build -t <tag> [URL/FILE] # Use "." for $PWD
# Pull an image from a registry
docker pull [IMAGE]
# Push an image to a registry
docker push [IMAGE]
# Create an image from a container
docker commit [CONTAINER] [NEW_IMAGE_NAME]
# Show the history of an image
docker history [IMAGE]
# Remove an image
docker rmi [IMAGE]
# Save an image to a tar archive
docker save [IMAGE] > [TAR_FILE]
# Create an image from a tarball
docker import [URL/FILE]
# Load an image from a tar archive or stdin 
docker load [TAR_FILE/STDIN_FILE]
```
---

### Containers

```sh
# Run a new container.
docker run [IMAGE]
# Rename an existing container.
docker rename [CONTAINER_NAME] [NEW_CONTAINER_NAME]
# Start a container and keep it running in the background (detached).
docker run -d [IMAGE]
# Start a container and creates an interactive bash shell in the container.
docker run -it [IMAGE]
# Remove container after it exits.
docker run --rm [IMAGE]
# Execute command inside already running container.
docker exec -it [CONTAINER]
# Stop a running container through SIGTERM.
docker stop [CONTAINER]
# Stop a running container through SIGKILL.
docker kill [CONTAINER]
# Start a stopped container
docker start [CONTAINER]
# Delete a container (if it is not running).
docker rm [CONTAINER]
# Delete all running and stopped containers
docker container rm -f $(docker ps -aq)
# Update the configuration of the container.
docker update [CONTAINER]
```
---

### Network

```sh
# List networks
docker network ls
# Show information on one or more networks
docker network inspect [NETWORK]
# Connects a container to a network
docker network connect [NETWORK] [CONTAINER]
# Disconnect a container from a network
docker network disconnect [NETWORK] [CONTAINER]
# Remove one or more networks
docker network rm [NETWORK]
```
---

### Monitoring

```sh
# Lists both running and stopped containers
docker ps -a
# Show live resource statistics of containers
docker stats [CONTAINER]
# Show port mapping for a container
docker port [CONTAINER]
# List real-time events from a container
docker events [CONTAINER]
# List the logs from a running container
docker logs [CONTAINER]
# Show running processes in a container
docker top [CONTAINER]
# Show changes on the filesystem since creation
docker diff [CONTAINER]
```






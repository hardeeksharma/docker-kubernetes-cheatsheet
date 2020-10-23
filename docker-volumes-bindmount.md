# Docker Volumes & Bind mounts Commands
Docker container are disposable. Means we can throw away a container and start a new one.
But there may be sometimes when our images produce some unique data and we want to persist it even if the container is disposed.
The data doesn’t persist when that container no longer exists, and it can be difficult to get the data out of the container if another process needs it.

Docker has two options for containers to store files in the host machine, so that the files are persisted even after the container stops: volumes, and bind mounts.

- **Volumes** are stored in a part of the host filesystem which is managed by Docker (/var/lib/docker/volumes/ on Linux). Non-Docker processes should not modify this part of the filesystem. Volumes are the best way to persist data in Docker.

- **Bind mounts** may be stored anywhere on the host system. They may even be important system files or directories. Non-Docker processes on the Docker host or a Docker container can modify them at any time.

### VOLUMES
1. Volumes outlive the container.
2. They need mannual deletion.
3. Volumes can be more safely shared among multiple containers.

Suppose we pull mysql image
`docker pull mysql`.\ 
Now inspect the image to see its configurations, there we will see the volume this image will use, `docker image inspect mysql`

```
"Volumes": {
    "/var/lib/mysql": {}
}
```

## Commands
---
- **List volumes** `docker volume ls`
- **Inspect a volume** `docker volume inspect [volume-hash/name]`
- **Create a volume** `docker volume create my-vol`
- **Remove a volume** `docker volume rm my-vol`



Source : https://docs.docker.com/storage/

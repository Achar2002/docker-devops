# Docker Useful Commands 🐳

## 1. Docker Installation and Status

```bash
docker --version
docker version
docker info
docker help

sudo systemctl status docker
sudo systemctl start docker
sudo systemctl stop docker
sudo systemctl restart docker
sudo systemctl enable docker
sudo systemctl is-active docker
```

## 2. Docker Image Commands

```bash
docker images
docker image ls
docker pull nginx
docker pull nginx:alpine
docker image inspect nginx
docker image history nginx
docker tag nginx my-nginx:v1
docker rmi nginx
docker image rm IMAGE_ID
docker image prune
docker image prune -a
```

## 3. Docker Container Commands

```bash
docker ps
docker ps -a
docker ps -q
docker ps -aq

docker create --name web nginx
docker run nginx
docker run -d --name web nginx
docker run -d --name web -p 8080:80 nginx
docker run -it ubuntu:24.04 bash
docker run --rm hello-world

docker start web
docker stop web
docker restart web
docker kill web
docker pause web
docker unpause web
docker rename web web-new
```

## 4. Remove Multiple Containers

### Remove specific containers

```bash
docker rm web api worker
```

### Force-remove multiple containers

```bash
docker rm -f web api worker
```

### Remove containers using IDs

```bash
docker rm -f ID1 ID2 ID3
```

### Remove all stopped containers

```bash
docker container prune
```

### Remove all exited containers

```bash
docker rm $(docker ps -aq --filter status=exited)
```

### Stop all running containers

```bash
docker stop $(docker ps -q)
```

### Remove all containers, running and stopped

```bash
docker rm -f $(docker ps -aq)
```

### Find containers by name

```bash
docker ps -a --filter name=web
```

### Find containers by image

```bash
docker ps -a --filter ancestor=nginx
```

### Remove containers created from a specific image

```bash
docker rm -f $(docker ps -aq --filter ancestor=nginx)
```

> **Warning:** The commands that use `docker rm -f` can delete running containers. Always check the target containers first using `docker ps -a`.

## 5. Execute Commands Inside Containers

```bash
docker exec -it web bash
docker exec -it web sh
docker exec web ls /
docker exec web env
docker exec -u root -it web sh
docker attach web
```

- `exec` starts a new process inside a running container.
- `attach` connects to the container's main process.

## 6. Docker Logs and Troubleshooting

```bash
docker logs web
docker logs -f web
docker logs --tail 100 web
docker logs --since 10m web

docker inspect web
docker top web
docker port web
docker stats
docker stats --no-stream
docker diff web
docker events
docker system df
```

## 7. Copy Files Between Host and Container

```bash
# Copy a host file into a container
docker cp ./file.txt web:/tmp/file.txt

# Copy a container file to the host
docker cp web:/tmp/file.txt ./file.txt
```

## 8. Dockerfile and Image Build Commands

```bash
docker build -t myapp:v1 .
docker build --no-cache -t myapp:v1 .
docker build -f Dockerfile.prod -t myapp:prod .
docker image history myapp:v1
```

### Common Dockerfile Instructions

| Instruction | Purpose |
|---|---|
| `FROM` | Select a base image |
| `WORKDIR` | Set the working directory |
| `COPY` | Copy files into the image |
| `RUN` | Execute commands during the build |
| `ENV` | Set environment variables |
| `ARG` | Define build-time arguments |
| `EXPOSE` | Document the intended container port |
| `USER` | Set the runtime user |
| `CMD` | Define the default command or arguments |
| `ENTRYPOINT` | Define the main executable |

## 9. Docker Hub Commands

```bash
docker login
docker logout
docker search nginx

docker tag myapp:v1 YOUR_DOCKERHUB_USERNAME/myapp:v1
docker push YOUR_DOCKERHUB_USERNAME/myapp:v1
docker pull YOUR_DOCKERHUB_USERNAME/myapp:v1
```

Never commit passwords, tokens, or Docker authentication files to GitHub.

## 10. Docker Networking

```bash
docker network ls
docker network create app-net
docker network inspect app-net

docker network connect app-net web
docker network disconnect app-net web
docker network rm app-net
docker network prune
```

### Port Mapping

```bash
docker run -d --name web -p 8080:80 nginx
docker port web
```

Host port `8080` maps to container port `80`.

## 11. Docker Volumes and Storage

```bash
docker volume ls
docker volume create app-data
docker volume inspect app-data
docker volume rm app-data
docker volume prune
```

### Run a Container with a Volume

```bash
docker run -d --name data-demo \
  -v app-data:/data alpine sleep 1d
```

Volumes can preserve data after a container is removed. Check before deleting them.

## 12. Docker Compose Commands

```bash
docker compose version
docker compose config

docker compose up
docker compose up -d
docker compose ps
docker compose logs
docker compose logs -f
docker compose exec web sh

docker compose stop
docker compose start
docker compose restart
docker compose down

docker compose build
docker compose pull
```

`docker compose down -v` also removes associated Compose-managed volumes.

## 13. Environment Variables and Resource Limits

```bash
docker run --rm -e APP_ENV=dev alpine env
docker run --rm --env-file .env alpine env

docker run -d --name limited --memory 512m nginx
docker run -d --name cpu-limited --cpus 1 nginx

docker stats
```

Do not commit `.env` files containing secrets to GitHub.

## 14. Docker Cleanup Commands

```bash
docker container prune
docker image prune
docker image prune -a
docker network prune
docker volume prune

docker system prune
docker system prune -a
```

- `docker container prune` removes stopped containers.
- `docker image prune` removes dangling images.
- `docker image prune -a` removes unused images.
- `docker system prune -a` performs broader cleanup.
- Volume deletion can permanently remove data.

## 15. Save, Load, Export and Import

```bash
docker save -o myapp.tar myapp:v1
docker load -i myapp.tar

docker export web -o web-filesystem.tar
docker import web-filesystem.tar imported-image:v1
```

- `save` and `load` preserve image layers and metadata.
- `export` and `import` operate on a container filesystem and do not preserve the original image history or complete configuration.

## 16. Docker Context Commands

```bash
docker context ls
docker context show
docker context inspect
```

These commands help inspect Docker contexts, which determine which Docker endpoint the CLI communicates with.

## 17. Daily DevOps Practice Commands

```bash
# Check containers
docker ps -a

# Check images
docker images

# Troubleshoot a container
docker logs CONTAINER
docker inspect CONTAINER

# Enter a running container
docker exec -it CONTAINER sh

# Monitor resources
docker stats

# Check storage usage
docker system df

# Check networking and storage
docker network ls
docker volume ls
```

## Official Documentation

- [Docker CLI Reference](https://docs.docker.com/reference/cli/docker/)
- [Docker Engine](https://docs.docker.com/engine/)
- [Docker Compose](https://docs.docker.com/compose/)
- [Dockerfile Reference](https://docs.docker.com/reference/dockerfile/)
- [Docker Hub](https://hub.docker.com/)

---

**Learning tip:** Practice each command in a test environment and document its purpose, output, and troubleshooting steps.

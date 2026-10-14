# Docker Useful Commands — Complete Cheat Sheet 🐳

A practical command reference for Docker learning, Linux/Ubuntu, AWS EC2, and DevOps labs.

> Replace placeholders such as `CONTAINER`, `IMAGE`, `YOUR_DOCKERHUB_USERNAME`, and ports with your own values. Be careful with `prune`, `rm -f`, and volume deletion commands because they can delete data.

## 1. Installation, version, and service

```bash
docker --version
docker version
docker info
docker help
docker COMMAND --help

sudo systemctl status docker
sudo systemctl start docker
sudo systemctl stop docker
sudo systemctl restart docker
sudo systemctl enable docker
sudo systemctl disable docker
sudo systemctl is-active docker
sudo journalctl -u docker
sudo journalctl -u docker --since "1 hour ago"
```

On Ubuntu, use the current official Docker Engine installation guide:
https://docs.docker.com/engine/install/ubuntu/

### Run Docker without `sudo` (Linux)
```bash
sudo usermod -aG docker "$USER"
```
Log out and back in (or start a new login session), then test with `docker ps`.
**Security:** membership in the `docker` group grants root-equivalent privileges.

## 2. Image commands

```bash
docker images
docker image ls
docker pull nginx
docker pull nginx:alpine
docker image inspect nginx:alpine
docker image history nginx:alpine
docker image tag nginx:alpine my-nginx:v1
docker image rm IMAGE_ID
docker rmi IMAGE
docker image prune
docker image prune -a
```

- `pull`: download an image.
- `tag`: add a name/tag to an image.
- `history`: show image layers/history.
- `inspect`: show detailed metadata.
- `prune`: delete unused images; review carefully.

## 3. Container lifecycle commands

```bash
docker ps
docker ps -a
docker ps -q
docker ps --no-trunc
docker create --name web nginx
docker run nginx
docker run -d --name web nginx
docker run --name web -d -p 8080:80 nginx
docker run --rm hello-world
docker run -it ubuntu:24.04 bash
docker start web
docker stop web
docker restart web
docker kill web
docker pause web
docker unpause web
docker rename web web-new
docker rm web
docker rm -f web
docker update --restart unless-stopped web
```

### Common `docker run` options

| Option | Meaning |
|---|---|
| `-d` | Run in the background |
| `-it` | Interactive terminal |
| `--name NAME` | Assign a container name |
| `-p HOST:CONTAINER` | Publish/map a host port to a container port |
| `-P` | Publish exposed ports to random host ports |
| `-e KEY=value` | Set an environment variable |
| `--env-file FILE` | Read environment variables from a file |
| `-v VOLUME:/path` | Mount a named volume or bind mount |
| `--mount ...` | Specify a mount explicitly |
| `--network NAME` | Connect to a Docker network |
| `--restart POLICY` | Set restart policy |
| `--memory 512m` | Set a memory limit |
| `--cpus 1` | Limit CPU usage |
| `--rm` | Remove container when it exits |
| `--read-only` | Use a read-only root filesystem (if app supports it) |

Example:
```bash
docker run -d --name nginx-web -p 8080:80 --restart unless-stopped nginx:alpine
```

## 4. Logs, inspect, and troubleshooting

```bash
docker logs CONTAINER
docker logs -f CONTAINER
docker logs --tail 100 CONTAINER
docker logs --since 10m CONTAINER
docker inspect CONTAINER
docker inspect --format '{{.State.Status}}' CONTAINER
docker stats
docker stats --no-stream
docker top CONTAINER
docker events
docker diff CONTAINER
docker port CONTAINER
docker wait CONTAINER
```

Useful troubleshooting sequence:
```bash
docker ps -a
docker logs CONTAINER
docker inspect CONTAINER
docker stats
```

## 5. Execute commands and copy files

```bash
docker exec -it CONTAINER bash
docker exec -it CONTAINER sh
docker exec CONTAINER ls /
docker exec CONTAINER env
docker exec -u root -it CONTAINER sh
docker attach CONTAINER
docker cp ./file.txt CONTAINER:/tmp/file.txt
docker cp CONTAINER:/tmp/file.txt ./file.txt
```

`exec` starts a new process inside a running container. `attach` connects to the container's main process.

## 6. Build images with Dockerfile

```bash
docker build .
docker build -t myapp:v1 .
docker build -f Dockerfile.prod -t myapp:prod .
docker build --no-cache -t myapp:v1 .
docker build --pull -t myapp:v1 .
docker image history myapp:v1
```

Example Dockerfile:
```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
```

Build and test:
```bash
docker build -t my-nginx:v1 .
docker run -d --name my-nginx -p 8080:80 my-nginx:v1
curl http://localhost:8080
```

Common Dockerfile instructions: `FROM`, `WORKDIR`, `COPY`, `ADD`, `RUN`, `ENV`, `ARG`, `EXPOSE`, `USER`, `CMD`, `ENTRYPOINT`, `HEALTHCHECK`.

## 7. Docker Hub and registries

```bash
docker login
docker logout
docker search nginx
docker tag myapp:v1 YOUR_DOCKERHUB_USERNAME/myapp:v1
docker push YOUR_DOCKERHUB_USERNAME/myapp:v1
docker pull YOUR_DOCKERHUB_USERNAME/myapp:v1
```

Use a token where supported. Never commit credentials or Docker authentication files to Git.

## 8. Port mapping and networking

```bash
docker network ls
docker network create app-net
docker network inspect app-net
docker network connect app-net CONTAINER
docker network disconnect app-net CONTAINER
docker network rm app-net
docker network prune
```

Run on a user-defined bridge:
```bash
docker run -d --name web --network app-net nginx:alpine
docker run --rm -it --network app-net alpine sh
```

Publish host port 8080 to container port 80:
```bash
docker run -d --name web -p 8080:80 nginx
docker port web
```

Common network drivers include `bridge`, `host`, `none`, and (in supported multi-host setups) `overlay`. `EXPOSE` documents a container port; it does not publish the port by itself.

## 9. Volumes and mounts

```bash
docker volume ls
docker volume create app-data
docker volume inspect app-data
docker volume rm app-data
docker volume prune
```

Run a container with a named volume:
```bash
docker run -d --name data-demo -v app-data:/data alpine sleep 1d
```

Bind mount the current directory into `/work`:
```bash
docker run --rm -it --mount type=bind,src="$PWD",dst=/work alpine sh
```

Named volumes are Docker-managed. Bind mounts map a host path. Do not remove a volume unless you are sure its data is no longer needed.

## 10. Docker Compose

Run these from the directory containing `compose.yaml` or `docker-compose.yml`.

```bash
docker compose version
docker compose config
docker compose up
docker compose up -d
docker compose ps
docker compose logs
docker compose logs -f SERVICE
docker compose exec SERVICE sh
docker compose restart
docker compose stop
docker compose start
docker compose down
docker compose pull
docker compose build
```

`docker compose down -v` also removes Compose-managed volumes. Use it only when you intentionally want to delete that data.

Example `compose.yaml`:
```yaml
services:
  web:
    image: nginx:alpine
    ports:
      - "8080:80"
    restart: unless-stopped
```

## 11. Environment variables and resource limits

```bash
docker run --rm -e APP_ENV=dev alpine env
docker run --rm --env-file .env alpine env
docker run -d --name limited --memory 512m --cpus 1 nginx:alpine
docker stats
```

Do not put secrets in a Dockerfile or commit `.env` files containing credentials. Environment variables can be visible to users with sufficient Docker access.

## 12. Cleanup commands — use carefully

```bash
docker container prune
docker image prune
docker image prune -a
docker network prune
docker volume prune
docker system df
docker system prune
docker system prune -a
```

These remove unused resources. `docker system prune -a` is broader than basic prune and may remove unused images you intended to keep. Volumes are not removed by default with standard `docker system prune`; adding `--volumes` can delete unused volumes and their data.

## 13. Save, load, export, and import

```bash
docker save -o myapp-image.tar myapp:v1
docker load -i myapp-image.tar
docker export CONTAINER -o container-filesystem.tar
docker import container-filesystem.tar imported-image:v1
```

- `save` / `load`: preserve and restore images, including image metadata and layers.
- `export` / `import`: export a container filesystem and create an image from it; this does not preserve the original image history or full container configuration.

## 14. Health checks and restart policies

```bash
docker inspect CONTAINER
docker update --restart no CONTAINER
docker update --restart on-failure CONTAINER
docker update --restart unless-stopped CONTAINER
```

Restart policies include `no`, `on-failure`, `always`, and `unless-stopped`. A restart policy is not a substitute for monitoring or application health checks.

## 15. System information and disk usage

```bash
docker info
docker system df
docker stats
docker stats --no-stream
docker version
docker context ls
docker context show
docker context inspect
```

## 16. Common practice workflows

### Run Nginx and test it
```bash
docker pull nginx:alpine
docker run -d --name web -p 8080:80 nginx:alpine
curl http://localhost:8080
docker logs web
docker stop web
docker rm web
```

### Build, tag, and push your own image
```bash
docker build -t myapp:v1 .
docker tag myapp:v1 YOUR_DOCKERHUB_USERNAME/myapp:v1
docker login
docker push YOUR_DOCKERHUB_USERNAME/myapp:v1
```

### Debug a container that exited
```bash
docker ps -a
docker logs CONTAINER
docker inspect CONTAINER
```

## Official documentation

- Docker CLI reference: https://docs.docker.com/reference/cli/docker/
- Docker Engine: https://docs.docker.com/engine/
- Docker Compose: https://docs.docker.com/compose/
- Dockerfile reference: https://docs.docker.com/reference/dockerfile/
- Docker Hub: https://hub.docker.com/

---
Keep adding your own lab output, screenshots, errors, causes, and fixes as you practice.

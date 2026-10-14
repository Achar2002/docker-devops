# Docker Learning Journey 🐳

A hands-on Docker learning repository covering container fundamentals, commands, images, networking, storage, Dockerfiles, Compose, troubleshooting, and practical projects.

> These notes are written as a personal learning guide. Commands and examples should be tested in a lab environment before using them in production.

## Learning Roadmap

| No. | Topic | What you'll learn |
|---|---|---|
| 01 | [Docker Fundamentals](01-docker-fundamentals/README.md) | Containers, virtualization, Docker architecture, images vs containers |
| 02 | [Installation and Setup](02-installation-and-setup/README.md) | Install Docker Engine, verify installation, run first container |
| 03 | [Docker Commands](03-docker-commands/README.md) | Manage images, containers, logs, exec, inspect, cleanup |
| 04 | [Docker Images](04-docker-images/README.md) | Image layers, tags, registries, pull, push, build |
| 05 | [Dockerfile](05-dockerfile/README.md) | Instructions, build context, caching, best practices |
| 06 | [Container Lifecycle](06-container-lifecycle/README.md) | Create, start, stop, restart, remove, troubleshoot containers |
| 07 | [Docker Networking](07-docker-networking/README.md) | Bridge, host, none, custom networks, port publishing |
| 08 | [Docker Storage](08-docker-storage/README.md) | Volumes, bind mounts, tmpfs, persistence |
| 09 | [Docker Compose](09-docker-compose/README.md) | Multi-container applications using Compose |
| 10 | [Environment Variables and Secrets](10-environment-and-secrets/README.md) | Runtime configuration and safer secret handling |
| 11 | [Docker Logs and Troubleshooting](11-logs-and-troubleshooting/README.md) | Logs, inspect, resource usage, common failures |
| 12 | [Docker Security and Best Practices](12-security-and-best-practices/README.md) | Least privilege, non-root users, image scanning, small images |
| 13 | [Docker Hub and Registries](13-docker-hub-and-registries/README.md) | Tagging, authentication, pushing and pulling images |
| 14 | [Hands-on Labs](14-hands-on-labs/README.md) | Practice exercises and mini-projects |

## Prerequisites

- Basic Linux terminal commands
- Basic understanding of files, processes, ports, and networking
- A Linux machine or Ubuntu EC2 instance
- Docker Engine and Docker CLI

## Quick Start

Check the Docker installation:

```bash
docker --version
docker info
docker run hello-world
```

List running containers and all containers:

```bash
docker ps
docker ps -a
```

## Repository Structure

```text
Docker-Learning-Repo/
├── README.md
├── 01-docker-fundamentals/
├── 02-installation-and-setup/
├── 03-docker-commands/
├── 04-docker-images/
├── 05-dockerfile/
├── 06-container-lifecycle/
├── 07-docker-networking/
├── 08-docker-storage/
├── 09-docker-compose/
├── 10-environment-and-secrets/
├── 11-logs-and-troubleshooting/
├── 12-security-and-best-practices/
├── 13-docker-hub-and-registries/
└── 14-hands-on-labs/
```

## How I Use This Repository

1. Learn the concept.
2. Write the commands in my own words.
3. Run each command in a lab.
4. Save useful output or screenshots where appropriate.
5. Document errors, causes, and fixes.
6. Update the checklist as I complete each topic.

## Progress Tracker

- [ ] 01 — Docker Fundamentals
- [ ] 02 — Installation and Setup
- [ ] 03 — Docker Commands
- [ ] 04 — Docker Images
- [ ] 05 — Dockerfile
- [ ] 06 — Container Lifecycle
- [ ] 07 — Docker Networking
- [ ] 08 — Docker Storage
- [ ] 09 — Docker Compose
- [ ] 10 — Environment Variables and Secrets
- [ ] 11 — Logs and Troubleshooting
- [ ] 12 — Security and Best Practices
- [ ] 13 — Docker Hub and Registries
- [ ] 14 — Hands-on Labs

## Useful References

- [Docker Documentation](https://docs.docker.com/)
- [Docker Get Started](https://docs.docker.com/get-started/)
- [Docker Hub](https://hub.docker.com/)

---
Learning by doing, one container at a time. 🐳

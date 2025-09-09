# Docker Cheat Sheet

Essential Docker commands for containerization and deployment.

---

## Table of Contents
- [Basic Commands](#basic-commands)
- [Container Management](#container-management)
- [Image Management](#image-management)
- [Docker Compose](#docker-compose)
- [Networking](#networking)
- [Volumes](#volumes)
- [Dockerfile](#dockerfile)
- [System Information](#system-information)

---

## Basic Commands

| Command | Description | Example |
|---------|-------------|---------|
| `docker --version` | Show Docker version | `docker --version` |
| `docker info` | Show system information | `docker info` |
| `docker help` | Show help | `docker help` |

## Container Management

| Command | Description | Example |
|---------|-------------|---------|
| `docker run <image>` | Create and start container | `docker run nginx` |
| `docker run -d <image>` | Run in background (detached) | `docker run -d nginx` |
| `docker run -it <image>` | Interactive with terminal | `docker run -it ubuntu bash` |
| `docker run -p <host>:<container> <image>` | Port mapping | `docker run -p 8080:80 nginx` |
| `docker run --name <name> <image>` | Name container | `docker run --name my-nginx nginx` |
| `docker ps` | List running containers | `docker ps` |
| `docker ps -a` | List all containers | `docker ps -a` |
| `docker start <container>` | Start stopped container | `docker start my-nginx` |
| `docker stop <container>` | Stop running container | `docker stop my-nginx` |
| `docker restart <container>` | Restart container | `docker restart my-nginx` |
| `docker rm <container>` | Remove container | `docker rm my-nginx` |
| `docker rm -f <container>` | Force remove running container | `docker rm -f my-nginx` |

## Image Management

| Command | Description | Example |
|---------|-------------|---------|
| `docker images` | List local images | `docker images` |
| `docker pull <image>` | Download image | `docker pull ubuntu:20.04` |
| `docker build -t <name> .` | Build image from Dockerfile | `docker build -t my-app .` |
| `docker build -t <name> <path>` | Build from specific path | `docker build -t my-app ./app` |
| `docker rmi <image>` | Remove image | `docker rmi nginx` |
| `docker rmi -f <image>` | Force remove image | `docker rmi -f nginx` |
| `docker tag <image> <new-tag>` | Tag image | `docker tag my-app my-app:v1.0` |
| `docker push <image>` | Push to registry | `docker push my-app:v1.0` |
| `docker history <image>` | Show image layers | `docker history nginx` |

## Docker Compose

| Command | Description | Example |
|---------|-------------|---------|
| `docker-compose up` | Start services | `docker-compose up` |
| `docker-compose up -d` | Start in background | `docker-compose up -d` |
| `docker-compose down` | Stop and remove | `docker-compose down` |
| `docker-compose ps` | List services | `docker-compose ps` |
| `docker-compose logs` | View logs | `docker-compose logs` |
| `docker-compose logs <service>` | Service-specific logs | `docker-compose logs web` |
| `docker-compose build` | Build services | `docker-compose build` |
| `docker-compose restart` | Restart services | `docker-compose restart` |
| `docker-compose exec <service> <cmd>` | Execute command in service | `docker-compose exec web bash` |

## Networking

| Command | Description | Example |
|---------|-------------|---------|
| `docker network ls` | List networks | `docker network ls` |
| `docker network create <name>` | Create network | `docker network create my-network` |
| `docker network inspect <network>` | Inspect network | `docker network inspect bridge` |
| `docker run --network <network> <image>` | Connect to network | `docker run --network my-network nginx` |

## Volumes

| Command | Description | Example |
|---------|-------------|---------|
| `docker volume ls` | List volumes | `docker volume ls` |
| `docker volume create <name>` | Create volume | `docker volume create my-volume` |
| `docker volume inspect <volume>` | Inspect volume | `docker volume inspect my-volume` |
| `docker run -v <volume>:<path> <image>` | Mount volume | `docker run -v my-volume:/data nginx` |
| `docker run -v <host-path>:<container-path> <image>` | Bind mount | `docker run -v $(pwd):/app nginx` |

## Dockerfile

### Common Instructions
```dockerfile
FROM ubuntu:20.04                    # Base image
MAINTAINER author@example.com        # Author info
RUN apt-get update && apt-get install -y curl  # Execute commands
COPY . /app                          # Copy files from host
ADD file.tar.gz /app                 # Copy and extract
WORKDIR /app                         # Set working directory
ENV NODE_ENV=production              # Set environment variable
EXPOSE 8080                          # Expose port
CMD ["npm", "start"]                 # Default command
ENTRYPOINT ["./entrypoint.sh"]       # Always executed
USER www-data                        # Set user
VOLUME ["/data"]                     # Create mount point
```

## System Information

| Command | Description | Example |
|---------|-------------|---------|
| `docker exec -it <container> <cmd>` | Execute command in running container | `docker exec -it my-nginx bash` |
| `docker logs <container>` | View container logs | `docker logs my-nginx` |
| `docker logs -f <container>` | Follow log output | `docker logs -f my-nginx` |
| `docker inspect <container>` | Detailed container info | `docker inspect my-nginx` |
| `docker stats` | Live resource usage | `docker stats` |
| `docker system df` | Show disk usage | `docker system df` |
| `docker system prune` | Remove unused data | `docker system prune` |
| `docker system prune -a` | Remove all unused data | `docker system prune -a` |

## Advanced Operations

### Multi-stage Builds
```dockerfile
FROM node:16 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
```

### Health Checks
```dockerfile
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD curl -f http://localhost/ || exit 1
```

---

## Resources
- [Docker Documentation](https://docs.docker.com/)
- [Docker Hub](https://hub.docker.com/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)

---
*Originally compiled from various sources. Contributions welcome!*
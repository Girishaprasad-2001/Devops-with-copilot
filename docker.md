# Docker A to Z Commands Cheat Sheet

A comprehensive Docker command reference for DevOps Engineers, SREs, Developers, Docker Certified Associate (DCA) preparation, and production environments.

---

# A. Docker Installation & Version

## Check Docker Version

```bash
docker --version
```

```bash
docker version
```

## Docker Information

```bash
docker info
```

---

# B. Image Commands

## List Images

```bash
docker images
```

or

```bash
docker image ls
```

## Pull Image

```bash
docker pull nginx
```

## Search Image

```bash
docker search ubuntu
```

## Remove Image

```bash
docker rmi nginx
```

## Remove Multiple Images

```bash
docker rmi image1 image2
```

## Force Remove Image

```bash
docker rmi -f nginx
```

---

# C. Container Commands

## List Running Containers

```bash
docker ps
```

## List All Containers

```bash
docker ps -a
```

## Create Container

```bash
docker create nginx
```

## Run Container

```bash
docker run nginx
```

## Run Detached

```bash
docker run -d nginx
```

## Run Interactive

```bash
docker run -it ubuntu bash
```

## Assign Name

```bash
docker run -d --name web nginx
```

## Remove Container

```bash
docker rm container_id
```

## Force Remove

```bash
docker rm -f container_id
```

---

# D. Start / Stop Containers

## Start

```bash
docker start container_id
```

## Stop

```bash
docker stop container_id
```

## Restart

```bash
docker restart container_id
```

## Pause

```bash
docker pause container_id
```

## Unpause

```bash
docker unpause container_id
```

## Kill

```bash
docker kill container_id
```

---

# E. Execute Commands

## Open Bash Shell

```bash
docker exec -it container_id bash
```

## Open sh Shell

```bash
docker exec -it container_id sh
```

## Run Command

```bash
docker exec container_id ls
```

---

# F. Container Logs

## View Logs

```bash
docker logs container_id
```

## Follow Logs

```bash
docker logs -f container_id
```

## Last 100 Lines

```bash
docker logs --tail 100 container_id
```

## With Timestamps

```bash
docker logs -t container_id
```

---

# G. Inspect Commands

## Inspect Container

```bash
docker inspect container_id
```

## Inspect Image

```bash
docker inspect nginx
```

## Get Container IP

```bash
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container_id
```

---

# H. Stats & Monitoring

## Resource Consumption

```bash
docker stats
```

## Specific Container

```bash
docker stats nginx
```

## Running Processes

```bash
docker top container_id
```

---

# I. Volumes

## List Volumes

```bash
docker volume ls
```

## Create Volume

```bash
docker volume create myvol
```

## Inspect Volume

```bash
docker volume inspect myvol
```

## Remove Volume

```bash
docker volume rm myvol
```

## Mount Volume

```bash
docker run -v myvol:/data nginx
```

---

# J. Network Commands

## List Networks

```bash
docker network ls
```

## Create Network

```bash
docker network create mynet
```

## Inspect Network

```bash
docker network inspect mynet
```

## Connect Container

```bash
docker network connect mynet nginx
```

## Remove Network

```bash
docker network rm mynet
```

---

# K. Build Images

## Build Image

```bash
docker build -t myapp:v1 .
```

## Build Using Dockerfile

```bash
docker build -f Dockerfile .
```

## No Cache Build

```bash
docker build --no-cache -t myapp .
```

---

# L. Tag Images

## Create Tag

```bash
docker tag myapp:v1 myrepo/myapp:v1
```

## List Tags / Images

```bash
docker images
```

---

# M. Docker Registry

## Login

```bash
docker login
```

## Logout

```bash
docker logout
```

## Push Image

```bash
docker push myrepo/myapp:v1
```

## Pull Image

```bash
docker pull myrepo/myapp:v1
```

---

# N. Save & Load Images

## Save Image

```bash
docker save -o nginx.tar nginx
```

## Load Image

```bash
docker load -i nginx.tar
```

---

# O. Copy Files

## Host → Container

```bash
docker cp file.txt container:/tmp/
```

## Container → Host

```bash
docker cp container:/tmp/file.txt .
```

---

# P. Prune (Cleanup)

## Remove Stopped Containers

```bash
docker container prune
```

## Remove Unused Images

```bash
docker image prune
```

## Remove All Unused Objects

```bash
docker system prune
```

## Remove Everything

```bash
docker system prune -a
```

## Remove Volumes Too

```bash
docker system prune -a --volumes
```

---

# Q. Docker Compose

## Start Services

```bash
docker compose up
```

## Detached Mode

```bash
docker compose up -d
```

## Stop Services

```bash
docker compose down
```

## View Running Services

```bash
docker compose ps
```

## Logs

```bash
docker compose logs
```

## Follow Logs

```bash
docker compose logs -f
```

## Scale Services

```bash
docker compose up --scale web=3
```

---

# R. Resource Limits

## CPU Limit

```bash
docker run --cpus=2 nginx
```

## Memory Limit

```bash
docker run -m 512m nginx
```

## CPU and Memory

```bash
docker run --cpus=2 -m 1g nginx
```

---

# S. Environment Variables

## Pass Variable

```bash
docker run -e ENV=prod nginx
```

## Multiple Variables

```bash
docker run -e APP=web -e PORT=8080 nginx
```

## Env File

```bash
docker run --env-file app.env nginx
```

---

# T. Port Mapping

## Publish Port

```bash
docker run -p 8080:80 nginx
```

## Multiple Ports

```bash
docker run -p 80:80 -p 443:443 nginx
```

## Random Port

```bash
docker run -P nginx
```

---

# U. User Management

## Run As User

```bash
docker run -u 1000 nginx
```

## Display User

```bash
docker exec container_id whoami
```

---

# V. View Events

## Docker Events

```bash
docker events
```

## Filter Events

```bash
docker events --filter container=nginx
```

---

# W. Docker Swarm Commands

## Initialize Swarm

```bash
docker swarm init
```

## Join Worker

```bash
docker swarm join
```

## List Nodes

```bash
docker node ls
```

## Create Service

```bash
docker service create nginx
```

## List Services

```bash
docker service ls
```

## Scale Service

```bash
docker service scale web=5
```

---

# X. Container Export & Import

## Export Container

```bash
docker export container_id > container.tar
```

## Import Container

```bash
docker import container.tar customimage:v1
```

---

# Y. System Information

## Disk Usage

```bash
docker system df
```

## Detailed Disk Usage

```bash
docker system df -v
```

## Docker Root Directory

```bash
docker info | grep "Docker Root Dir"
```

---

# Z. Advanced Production Commands

## Show Container IDs

```bash
docker ps -q
```

## Stop All Containers

```bash
docker stop $(docker ps -q)
```

## Remove All Containers

```bash
docker rm $(docker ps -aq)
```

## Remove Dangling Images

```bash
docker image prune
```

## Execute in Running Container

```bash
docker exec -it $(docker ps -q | head -1) bash
```

## Check Port Mapping

```bash
docker port container_id
```

## Check Container Health

```bash
docker inspect --format='{{json .State.Health}}' container_id
```

---

# Top 25 Docker Commands Used Daily

```bash
docker ps
docker ps -a
docker images
docker pull nginx
docker run -d nginx
docker run -it ubuntu bash
docker exec -it container bash
docker logs container
docker logs -f container
docker inspect container
docker stop container
docker start container
docker restart container
docker rm container
docker rmi image
docker build -t app:v1 .
docker tag app:v1 repo/app:v1
docker push repo/app:v1
docker network ls
docker volume ls
docker stats
docker system df
docker system prune -a
docker compose up -d
docker compose down
```

---

# Docker Troubleshooting Commands

```bash
docker ps -a
docker logs <container>
docker inspect <container>
docker stats
docker system df
docker events
docker exec -it <container> sh
docker network inspect bridge
docker volume inspect <volume>
docker system prune -a
```

---

# Summary

This cheat sheet covers the most important Docker commands used in:

- Docker Fundamentals
- Container Lifecycle Management
- Images & Registries
- Networking & Volumes
- Docker Compose
- Docker Swarm
- Troubleshooting & Monitoring
- DevOps & CI/CD Pipelines
- Kubernetes Administration
- Cloud Deployments
- Production Support Environments

⭐ Keep this README as a quick Docker reference guide for daily operations.

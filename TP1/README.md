# TP1 basics

## Network

```bash
docker network create app-network
```

## Postgresql

```bash
docker image build . -t tp1-db-img:latest
docker container run -d --rm -p 5432:5432 --network app-network -e POSTGRES_PASSWORD=pwd -e POSTGRES_USER=usr -v /home/leon/Documents/CPE/devops-ci-cd/TP1/database/data:/var/lib/postgresql/data --name tp1-db tp1-db-img:latest
```

## Adminer

```bash
docker run --network app-network -p 8081:8080 adminer
```

## Backend API

```bash
docker image build . -t tp1-backend-img:latest
docker run --network app-network -p 8082:8080 tp1-backend-img:latest
```

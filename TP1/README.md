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

[Lien Adminer](http://localhost:8081/?pgsql=tp1-db&username=usr&db=db&ns=public)

## Backend API

```bash
docker image build . -t tp1-backend-img:latest

docker run --network app-network -p 8082:8080 -e POSTGRES_PASSWORD=pwd -e POSTGRES_USER=usr tp1-backend-img:latest
```

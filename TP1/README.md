# TP1 Devops

[TP link](http://school.pages.takima.io/devops-resources/ch1-discover-docker-tp/)

## Network

Creates a Docker network named `app-network`. Docker networks allow containers to communicate with each other.

```bash
docker network create app-network
```

## Postgresql

Docker Hub image [here](https://hub.docker.com/repository/docker/leondumestre/tp-devops-database)

Builds a Docker image for PostgreSQL using the provided Dockerfile.
Runs a PostgreSQL container named `database`, exposing port `5432` connecting it to the `app-network`, setting environment variables for the PostgreSQL password and user, and mounting a local data directory for persistent storage.

`CreateScheme.sql` and `InsertData.sql` are copied to `docker-entrypoint-initdb.d` in the container to be executed during initialization.

```bash
docker image build . -t database-img:latest

docker run --rm -p 5432:5432 --network app-network -e POSTGRES_PASSWORD=pwd -e POSTGRES_USER=usr -v data:/var/lib/postgresql/data --name database database-img:latest
```

## Adminer

Runs an Adminer container on the `app-network`, exposing port 8000 for web access to Adminer. It allows to access to postgres database.
No need to build an image, `docker run` command fetch the latest adminer image online.

```bash
docker run --network app-network -p 8000:8080 adminer
```

[Adminer link](http://localhost:8000/?pgsql=tp1-db&username=usr&db=db&ns=public)

## Backend API

Docker Hub image [here](https://hub.docker.com/repository/docker/leondumestre/tp-devops-backend)

Builds a Docker image for the Backend API using the provided Dockerfile that incorporates a Spring framework-based application.

Runs a container named `backend`, exposing port `8080`, connecting it to the app-network, and setting environment variables for the PostgreSQL password and user required by the Spring application.

```bash
docker image build . -t backend-img:latest

docker run --network app-network -p 8080:8080 -e POSTGRES_PASSWORD=pwd -e POSTGRES_USER=usr --name backend backend-img:latest
```

[Backend API link](http://localhost:8080/)

Multiplestage build is a technique to make Docker images smaller. It separates the image creation stages from its execution. This allows the removal of temporary build elements that are not necessary for the application's runtime, thus reducing the final image size. This improves storage and transfer efficiency and enhances the security of the Docker image.

## Http Server

Docker Hub image [here](https://hub.docker.com/repository/docker/leondumestre/tp-devops-httpd)

Builds a Docker image for an HTTP server using the provided Dockerfile.
Runs a container named `httpd`, exposing port `80` and connecting it to the `app-network`.

Additionally, a reverse proxy is implemented to redirect traffic to the Backend API. To do this, a custom configuration with reverse proxy (`httpd-reverse-proxy.conf`) is added to the container to overwrite the current configuration.

```bash
docker image build . -t httpd-img:latest

docker run --rm -p 80:80 --network app-network --name httpd httpd-img:latest
```

[Http Server link](http://localhost:80/)

## Docker Compose

The `docker-compose` file defines 3 containers:

- **PostgreSQL Database (database)**: PostgreSQL image, exposes port 5432, mounts a local volume for persistent data storage, and sets environment variables for the PostgreSQL user and password.
- **Backend API (backend)**: Custom image for the Backend API, exposes port 8080, connects to the app-network, depends on the availability of the database service, and sets environment variables specifying the database connection details.
- **HTTP Server (httpd)**: Custom image for an HTTP server, exposes port 80, connects to the app-network, and depends on the availability of the backend service.

The services are interconnected through the `app-network`. The `depends_on` directive ensures proper startup order, and environment variables are used to configure database connections within the services.

## Docker Hub

At the end of the project, I published the images on Docker Hub after creating a tag locally.
This made it possible to replace the builds in the docker compose with the remote images.

```bash
docker tag database leondumestre/tp-devops-database:1.0

docker push leondumestre/tp-devops-database:1.0  
```

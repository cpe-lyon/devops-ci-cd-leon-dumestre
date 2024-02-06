# TP2 Devops

## Tests

`Testcontainers` is a Java library that makes it easy to set up and tear down Docker containers for integration testing. It provides lightweight, disposable instances of databases, web browsers, and other services needed for testing. This helps developers write reliable integration tests without complex setup or dependencies.

We can run tests locally before automating them.

```bash
mvn clean verify
```

## Github Actions

GitHub Actions is a built-in automation service provided by GitHub.

GitHub Actions is a powerful tool for automating various aspects of the software development lifecycle, improving efficiency, quality, and collaboration within development teams.

### Test backend

#### Workflow

- **Triggers:** This workflow runs on push events to the main and develop branches, as well as pull requests.
- **Purpose:** The purpose of this workflow is to test the backend codebase using Maven and SonarCloud.
- **Jobs:**
  - `test-backend`: This job sets up the JDK, caches dependencies, builds and analyzes the backend codebase using Maven and SonarCloud.

#### Job `test-backend`

- Checkout repository.
- Set up JDK 17.
- Cache SonarCloud packages.
- Cache Maven packages.
- Build and analyze backend codebase using Maven and SonarCloud.

#### SonarCloud

SonarCloud is a static code analysis tool that helps developers maintain code quality by identifying bugs, vulnerabilities, and code smells in their codebase.

When the job runs, SonarCloud scans the code and shows the results in a project created on the SonarCloud website.
To push the analysis to this project, we need to add the `SONAR_TOKEN` secret variable on GitHub.

This project analysis can be found [here](https://sonarcloud.io/project/overview?id=cpe-devops-leon-dumestre).

### Image

#### Workflow

- **Triggers:** This workflow runs when the workflow run of "CI devops - Test Backend" completes successfully on the `main` branch.
- **Purpose:** The purpose of this workflow is to build Docker images for the backend, database, and httpd services and push them to DockerHub.
- **Jobs:**
  - `build-and-push-docker-image`: This job checks out the repository, logs in to DockerHub, builds Docker images for the backend, database, and httpd services, and pushes them to DockerHub.

#### Job `build-and-push-docker-image`

- Checkout repository.
- Login to DockerHub.
- Build and push Docker images for backend, database, and httpd services to DockerHub.

#### Docker hub

When the job runs, images are built and pushed on Docker Hub:

- [backend](https://hub.docker.com/repository/docker/leondumestre/tp-devops-backend)
- [database](https://hub.docker.com/repository/docker/leondumestre/tp-devops-database)
- [httpd](https://hub.docker.com/repository/docker/leondumestre/tp-devops-httpd)

For that, we defined secret variables on GitHub. Thanks to that, `.yml` files can access these variables:

- `DOCKERHUB_USERNAME`
- `DOCKERHUB_TOKEN`

Username and token had been found in Docker hub website.


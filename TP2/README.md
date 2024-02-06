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

- **Triggers:** This workflow runs on push events to the main and develop branches, as well as pull requests.
- **Purpose:** The purpose of this workflow is to test the backend codebase using Maven and SonarCloud. Sonarcloud analyze can be found [here](https://sonarcloud.io/project/overview?id=cpe-devops-leon-dumestre).
- **Jobs:**
  - `test-backend`: This job sets up the JDK, caches dependencies, builds and analyzes the backend codebase using Maven and SonarCloud.

---

**`test-backend` steps**:

- Checkout repository.
- Set up JDK 17.
- Cache SonarCloud packages.
- Cache Maven packages.
- Build and analyze backend codebase using Maven and SonarCloud.

### Image

- **Triggers:** This workflow runs when the workflow run of "CI devops - Test Backend" completes successfully on the `main` branch.
- **Purpose:** The purpose of this workflow is to build Docker images for the backend, database, and httpd services and push them to DockerHub.
- **Jobs:**
  - `build-and-push-docker-image`: This job checks out the repository, logs in to DockerHub, builds Docker images for the backend, database, and httpd services, and pushes them to DockerHub.

---

**`build-and-push-docker-image` steps**:

- Checkout repository.
- Login to DockerHub.
- Build and push Docker images for backend, database, and httpd services to DockerHub.

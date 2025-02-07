# demo app - developing with Docker and Gitlab CI/CD

<img width="500" alt="image" src="https://user-images.githubusercontent.com/9394918/167876466-2c530828-d658-4efe-9064-825626cc6db5.png">

This repository contains the build and release code for the [online store](https://gitlab.praktikum-services.ru/Stasyan/momo-store).

This stage is the first stage of a full application build-delivery cycle, using CI/CD practices.

The application delivery and infrastructure deployment stage is described [here](https://github.com/wkwwa/store-cd)

CI is implemented through a [Downstream Pipeline](.gitlab-ci.yml) and contains 2 child pipelines: [backend](backend/.gitlab-ci.yml) и [frontend](frontend/.gitlab-ci.yml), launching a multi-stage build and release of Docker images [backend](backend/Dockerfile) и [frontend](frontend/Dockerfile) to a private Gitlab Registry.
 
The CI includes Semgrep SAST static analysis. 

Artifact building is done with SemVer versioning.

## To start the application from registry

All components are docker-based: [install Docker](https://docs.docker.com/engine/install/)

Step 1. Start backend first
```bash
docker network create -d bridge momo_network
docker run --rm -d --name momo-backend --network=momo_network registry.gitlab.com/devops3761117/momo-store/momo-backend:latest
```

Step 2. Start frontend
```bash
docker run --rm -d --name momo-frontend --network=momo_network -p 80:80 registry.gitlab.com/devops3761117/momo-store/momo-frontend:latest
```

Step 3: Access you application UI from browser
```bash
http://localhost:80
```

## To start the application locally

Step 1. Build the image. The dot "." at the end of the command denotes location of the Dockerfile
```bash
docker build -t momo-frontend .
docker build -t momo-backend .
```

Step 2. Create docker network
```bash
docker network create -d bridge momo_network
```

Step 3. Start backend first
```bash
docker run --rm -d --name momo-backend --network=momo_network momo-backend
```

Step 4. Start frontend
```bash
docker run --rm -d --name momo-frontend --network=momo_network -p 80:80 momo-frontend
```

Step 5. Access you application UI from browser 
```bash
http://localhost:80
```
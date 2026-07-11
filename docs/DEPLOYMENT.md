# Deployment Guide

## Overview
This project supports multiple deployment styles:
- local Docker Compose for development,
- remote Docker Compose with Traefik for production-like environments,
- Amazon ECR for container image storage,
- and Kubernetes as a future-ready hosting option.

## Prerequisites
- Docker Engine
- Docker Compose v2
- A domain name and DNS entries for subdomains
- Optional: AWS CLI and access to Amazon ECR
- Optional: a Kubernetes cluster and kubectl

## Docker Compose Deployment
1. Copy the repository to the target server.
2. Create a `.env` file with production values.
3. Start Traefik if you are exposing services publicly.
4. Deploy the stack:

```bash
docker compose -f compose.yml build
docker compose -f compose.yml up -d
```

## Traefik Setup
Traefik is used to route requests to the frontend and API based on hostnames.
The deployment files assume a shared Docker network named `traefik-public`.

Example public routes:
- `https://dashboard.example.com`
- `https://api.example.com`
- `https://adminer.example.com`
- `https://traefik.example.com`

## AWS ECR
The repository includes a GitHub Actions workflow that builds and pushes images to Amazon ECR.

### Required GitHub Variables and Secrets
- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `AWS_REGION`
- `ECR_REGISTRY`

### Example Commands
```bash
aws ecr create-repository --repository-name fullstack-fastapi-backend
aws ecr create-repository --repository-name fullstack-fastapi-frontend
```

## Kubernetes Deployment
A Kubernetes deployment is not yet fully templated in this repository, but the recommended pattern is:
- one `Deployment` for the FastAPI backend,
- one `Deployment` for the frontend,
- a `Service` for each component,
- a `Secret` for environment variables,
- and an `Ingress` or load balancer for external access.

### Example Backend Deployment Manifest
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: fastapi-backend
spec:
  replicas: 2
  selector:
    matchLabels:
      app: fastapi-backend
  template:
    metadata:
      labels:
        app: fastapi-backend
    spec:
      containers:
        - name: backend
          image: YOUR_ECR_IMAGE:latest
          ports:
            - containerPort: 8000
          envFrom:
            - secretRef:
                name: app-secrets
```

## CI/CD with GitHub Actions
The repository contains workflows for:
- building and pushing Docker images,
- deploying to staging,
- deploying to production,
- and running Playwright tests.

## Rollback Strategy
- Revert the Git commit or release.
- Redeploy the previous image tag.
- Recreate containers or roll back the deployment object in Kubernetes.

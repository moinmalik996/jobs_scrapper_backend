
# jobs_scrapper_backend

Cloud-native backend for a job scraping platform built with FastAPI and deployed on AWS. The system exposes authenticated APIs for job management and image-related workflows, runs scheduled background scraper tasks on ECS/Fargate, and ships through a CI/CD pipeline using AWS CodeBuild and CodePipeline.

## Overview

This repository contains the backend service for a job scraper platform. It is designed for containerized deployment and production hosting on AWS.

Core responsibilities:

- Serve FastAPI endpoints for authentication, job workflows, image uploads, and S3-related operations.
- Persist application data in PostgreSQL.
- Store uploaded assets in S3 in production.
- Run scheduled background scraping tasks twice daily using AWS scheduling and ECS/Fargate workloads.
- Deploy backend containers through CI/CD with rolling updates on an ECS cluster.
- Provide operational visibility through CloudWatch logging and monitoring.

## Project Scope

### Backend

- FastAPI-based REST API.
- Authentication and protected routes.
- Job listing, filtering, and user job action tracking.
- Image upload, retrieval, and analysis workflows.
- Alembic-based database schema migrations.

### Frontend Integration

- Supports a React frontend through API-based integration.
- Configured with CORS support for local and deployed frontend clients.

### Cloud Services

- AWS ECS for container orchestration.
- AWS Fargate for serverless container execution.
- AWS ECR for container image storage.
- AWS S3 for asset storage.
- AWS RDS/PostgreSQL for persistent relational data.
- AWS Lambda for supporting serverless workflows where needed.
- AWS EventBridge for scheduled scraper execution.

### CI/CD and Release Engineering

- AWS CodeBuild builds and pushes backend container images.
- AWS CodePipeline automates delivery to the target environment.
- ECS rolling deployment strategy enables low-downtime releases.
- Container image definitions are generated for deployment automation.

### Monitoring and Observability

- CloudWatch logs for runtime visibility.
- Metrics and alarms can be attached to API and scheduled job workloads.
- Centralized monitoring for deployment health and scraper execution status.

## High-Level Architecture

1. Developers push backend changes to the repository.
2. CodePipeline triggers CodeBuild.
3. CodeBuild builds the Docker image and pushes it to ECR.
4. ECS services pull the updated image and deploy using rolling updates.
5. Scheduled scraper jobs run on ECS/Fargate twice daily.
6. Application data is stored in PostgreSQL and files/assets are stored in S3.
7. Logs and operational telemetry are collected through CloudWatch.

## Technology Stack

- Backend: Python, FastAPI, SQLModel/SQLAlchemy, Alembic
- Frontend Integration: React
- Database: PostgreSQL
- Containers: Docker
- AWS: ECS, Fargate, ECR, S3, RDS, Lambda, EventBridge, IAM, CloudWatch, CodeBuild, CodePipeline

## Local Development

### Prerequisites

- Python 3.12+
- Docker and Docker Compose
- PostgreSQL if running without containers

### Run with Docker Compose

```bash
docker-compose up --build
```

The backend is exposed on port `8000` and the local PostgreSQL instance is exposed on port `5432`.

### Run Migrations

Create a new revision:

```bash
alembic revision -m "your migration message"
```

Apply migrations:

```bash
alembic upgrade head
```

## Deployment Notes

- The Docker image is built using the project `Dockerfile`.
- The current build specification pushes an image to ECR and emits `imagedefinitions.json` for downstream deployment stages.
- ECS cluster deployments are intended to use a rolling update strategy.
- Scheduled scraper execution can be attached to EventBridge rules targeting ECS/Fargate tasks.

<!-- ## Repository Naming

The current repository name is `jobs_scrapper_backend`, which matches the broader project scope more closely than the earlier image-focused name.

The repository is now positioned as a cloud-native job scraping backend that supports scheduled ECS/Fargate workloads, API services, and CI/CD delivery on AWS.

Recommended naming options:

- `jobs_scrapper_backend`
- `job-scraper-backend`
- `job-platform-backend`

If image recognition remains a major feature rather than a secondary capability, use a broader product-oriented name instead of an image-specific one.

## Suggested Positioning

For documentation, CVs, and interviews, describe this as a cloud-native job scraper backend with scheduled ECS/Fargate workers, CI/CD on AWS CodePipeline, and production deployment on ECS. -->


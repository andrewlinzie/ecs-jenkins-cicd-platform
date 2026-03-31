# ECS Jenkins CI/CD Platform
## Overview

This project implements a fully automated container deployment platform on AWS using ECS Fargate and Jenkins. It eliminates manual deployment workflows by enabling repeatable, versioned, and scalable releases through Infrastructure as Code (Terraform) and CI/CD automation.

The system provisions infrastructure, builds containerized applications, and deploys them to a production-ready environment with zero manual intervention.

## Purpose
The goal of this project is to demonstrate how to:
- Build an end-to-end CI/CD pipeline for containerized applications
- Automate infrastructure provisioning and application deployment
- Deliver scalable, production-ready services with minimal operational overhead

This results in a system that is automated, consistent, and production-aligned.

## Architecture
### High-Level Flow
1. Users access the application via an Application Load Balancer (ALB)
2. ALB routes traffic to ECS services (frontend + backend)
3. Backend services communicate internally within the VPC
4. Jenkins (on EC2) orchestrates the CI/CD pipeline
5. Docker images are built and pushed to Amazon ECR
6. Terraform provisions and updates infrastructure

### Core Components
- ECS Fargate – Serverless container orchestration
- Jenkins (EC2) – CI/CD control plane
- ECR – Container image registry
- ALB – Traffic routing + health checks
- VPC – Networking isolation (public/private subnets)
- IAM – Secure service-to-service access
- Security Groups – Controlled network communication

## Workflow
### CI/CD Workflow
1. Code is pushed to repository
2. Jenkins pipeline is triggered
3. Application is built and tested
4. Docker images are built
5. Images are pushed to ECR
6. ECS services are updated with new task definitions

## Tech Stack
- Terraform – Infrastructure as Code
- Docker – Containerization
- Jenkins – CI/CD automation
- AWS ECS (Fargate) – Container orchestration
- Amazon ECR – Image storage
- NGINX – Frontend container serving

## Key Engineering Decisions
### ECS (Fargate) vs EKS
ECS Fargate was chosen to minimize operational overhead and accelerate delivery.
- No Kubernetes control plane or node management
- Built-in scaling and orchestration
- Faster time-to-deployment

EKS was avoided due to unnecessary complexity for this workload.

### Jenkins vs GitHub Actions
Jenkins was selected for maximum flexibility and control.
- Full customization of pipeline stages
- Direct integration with Docker, Terraform, ECS
- Runs on self-managed infrastructure

GitHub Actions was less suitable due to limitations in complex multi-stage workflows.

### EC2-hosted Jenkins
Jenkins runs on EC2 to enable:
- Full control over runtime environment
- Custom tooling (Docker, AWS CLI, plugins)
- Deep integration with AWS services

Managed CI/CD platforms were avoided due to reduced flexibility.

### Image Tagging Strategy
Images are tagged using unique build-based identifiers.
- Each deployment maps to a specific build
- Enables precise rollbacks
- Eliminates ambiguity from latest tags

## Infrastructure Highlights
- ECS Fargate for serverless container execution
- ALB for public access, routing, and health checks
- Security groups enforce ALB → ECS traffic boundaries
- Terraform provisions all infrastructure components
- IAM roles enable secure Jenkins → AWS interactions

## Notable Features
- End-to-end CI/CD pipeline (commit → production)
- Zero-downtime deployments via ECS rolling updates
- Immutable infrastructure and deployments
- Scalable container hosting with Fargate
- Fully reproducible environments via Terraform
 
 ## Deployment Workflow (End-to-End)
This system follows a structured DevOps lifecycle:
1. Local Validation
    - Run frontend + backend locally
    - Verify connectivity
2. Containerization
    - Dockerize frontend and backend
    - Validate containers locally
3. CI/CD Setup
    - Deploy Jenkins on EC2
    - Configure plugins, credentials, and IAM roles
4. Infrastructure Provisioning
    - Use Terraform to provision:
        - ECS cluster
        - ALB
        - Networking (VPC, subnets, security groups)
        - ECR repositories
5. Pipeline Execution
    - Jenkins builds images
    - Pushes to ECR
    - Applies Terraform updates
    - Deploys to ECS
6. Production Deployment
    - Services run on ECS Fargate
    - ALB routes traffic to containers
    - Application is publicly accessible
7. Validation & Scaling
    - Health checks via ALB
    - Monitoring via CloudWatch
    - Auto-scaling based on load

This aligns directly with a full DevOps lifecycle from local development → production deployment → scaling validation

## Outcomes
- Eliminated manual deployments
- Enabled consistent, repeatable releases
- Reduced operational overhead with serverless containers
- Established production-ready DevOps workflow

## Future Improvements
- Add GitOps pipeline (GitHub Actions alternative)
- Implement blue/green deployments
- Integrate Secrets Manager / Parameter Store
- Add observability (logs, metrics dashboards)

## Summary
This project demonstrates a production-grade CI/CD system that integrates:
- Infrastructure as Code (Terraform)
- Containerization (Docker)
- Orchestration (ECS Fargate)
- Automation (Jenkins)

It reflects real-world DevOps practices: automation, scalability, reliability, and traceability.
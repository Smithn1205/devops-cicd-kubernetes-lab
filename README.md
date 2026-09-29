# DevOps CI/CD Kubernetes Lab

Hands-on DevOps lab with GitLab, Jenkins, Docker and Kubernetes, focused on CI/CD, containerization and automated deployment.

> **Status:** In progress — this repository documents the implementation as it is built.

## Overview

This project demonstrates a simple CI/CD workflow that takes application code from source control through automated build and containerization, and ultimately deploys it to Kubernetes.

### Planned workflow

```
Developer
   |
   v
GitLab
   |
   v
Jenkins
   |
   +--> Build / Test
   |
   +--> Docker Image
   |
   v
Kubernetes
   |
   v
Running Application
```

## Technologies

- GitLab — source control
- Jenkins — CI/CD automation
- Docker — containerization
- Kubernetes — container orchestration

## Project Goals

- Build a working CI/CD pipeline
- Understand Jenkins pipeline configuration
- Containerize an application with Docker
- Deploy and manage the application with Kubernetes
- Document the architecture and implementation
- Extend the pipeline with security controls as the project develops

## Repository Structure

The structure will be expanded as the implementation progresses:

```
.
├── README.md
├── app/
├── docker/
├── jenkins/
├── kubernetes/
├── screenshots/
└── docs/
```

## Implementation

Detailed implementation steps, configuration files, screenshots and troubleshooting notes will be added as each part of the lab is completed.

## Future DevSecOps Improvements

Once the core CI/CD pipeline is working, the project may be extended with security controls such as:

- Container image scanning
- Dependency scanning
- Secret detection
- Security gates in the CI/CD pipeline
- Kubernetes security hardening

These items will only be documented as implemented once they have been tested in the project.

## Learning Outcomes

This project is intended to build practical understanding of:

- CI/CD pipelines
- Jenkins automation
- Docker containerization
- Kubernetes deployments
- Cloud/infrastructure workflows
- DevSecOps concepts

## Disclaimer

This is a personal hands-on learning project and is not presented as professional production experience.

# DevOps CI/CD Kubernetes Lab

Hands-on DevOps lab using **GitLab, Jenkins, Docker and Kubernetes**, focused on CI/CD automation, containerization, deployment, troubleshooting and DevSecOps fundamentals.

> **Status:** Active hands-on lab — the repository documents features that have actually been implemented and tested.

## Overview

This project demonstrates a practical CI/CD workflow:

```
GitLab
   |
   v
Jenkins
   |
   +--> Docker-based Jenkins agent
   |
   +--> Build / Test
   |
   +--> Credentials handling
   |
   +--> Kubernetes connection
   |
   +--> Kubernetes deployment
   |
   +--> Artifact archiving
   |
   v
Kubernetes
   |
   v
Running application
```

The lab was built incrementally to understand how source control, CI/CD automation, containers and Kubernetes fit together.

## Technologies

- **GitLab** — source control and pipeline source
- **Jenkins** — CI/CD automation
- **Docker** — containers and Jenkins build agents
- **Kubernetes** — deployment and orchestration
- **kubectl** — Kubernetes administration and troubleshooting
- **Python** — application and test workload
- **Trivy** — container vulnerability scanning
- **Linux/Bash** — automation and troubleshooting

## Implemented

### 1. Docker containerization

The Python system-monitor application was containerized and tested with versioned images.

Examples:

```bash
docker build -t system-monitor:1.1 .
docker run --rm system-monitor:1.1
docker images
docker logs <container>
```

The lab also demonstrated container inspection, image history, container lifecycle commands and basic troubleshooting.

### 2. Jenkins CI/CD pipeline

A Jenkins Pipeline was created using a `Jenkinsfile`.

The pipeline includes:

- Build stage
- Python application test
- Docker environment verification
- Kubernetes client verification
- Kubernetes API connection test
- Kubernetes deployment
- Jenkins credentials test
- Artifact archiving
- Success/failure handling

Simplified flow:

```
Build
  ↓
Test
  ↓
Kubernetes connection test
  ↓
Deploy
  ↓
Credentials test
  ↓
Archive test output
```

### 3. Docker-based Jenkins agents

Jenkins was configured to provision a Docker-based agent.

The custom agent includes:

- Python
- Docker CLI
- kubectl
- curl

This demonstrates the Jenkins controller/agent model and allows pipeline work to run inside an isolated container environment.

### 4. Kubernetes deployment

The application was deployed to a local Kubernetes cluster.

The deployment was configured with **2 replicas** and verified using:

```bash
kubectl get nodes
kubectl get deployments
kubectl get pods
kubectl rollout status deployment/system-monitor
```

A Kubernetes deployment update was also tested to demonstrate a rolling update.

### 5. Kubernetes troubleshooting: CrashLoopBackOff

A deliberate container lifecycle problem was investigated.

The first application image completed its Python process and exited normally. Kubernetes expected the container to remain running, so it repeatedly restarted the container and eventually reported:

```
CrashLoopBackOff
```

The troubleshooting process was:

```
kubectl get pods
        ↓
kubectl logs <pod>
        ↓
identify application/container behaviour
        ↓
fix the image
        ↓
build a new image
        ↓
update the Deployment
        ↓
verify the Pods
```

The important lesson was that **CrashLoopBackOff does not automatically mean the application crashed**. In this case, the main process exited successfully while Kubernetes expected a long-running process.

### 6. Jenkins credentials

Jenkins credentials handling was tested using a Jenkins credential ID and `withCredentials`.

The pipeline verifies that the credential is available without printing the secret value.

Example pattern:

```groovy
withCredentials([
    string(credentialsId: 'demo-secret', variable: 'MY_SECRET')
]) {
    sh '''
        test -n "$MY_SECRET"
    '''
}
```

### 7. Artifact archiving and fingerprinting

The pipeline archives its test output:

```groovy
archiveArtifacts artifacts: 'test_output.txt', fingerprint: true
```

This demonstrates:

- Jenkins artifact archiving
- Artifact traceability
- Fingerprinting for identifying/tracking the exact artifact

**Important distinction:** this stores the artifact in Jenkins. It is not an Artifactory upload.

### 8. Container vulnerability scanning

Trivy was used to scan the container image for **HIGH** and **CRITICAL** vulnerabilities:

```bash
trivy image --severity HIGH,CRITICAL system-monitor:1.1
```

The scan produced vulnerability findings in the base image/Python environment.

The investigation included checking whether reported Python packages were actually installed directly in the application image and distinguishing direct dependencies, transitive/vendored components and base-image findings.

The key security lesson was:

```
Scan
  ↓
Investigate
  ↓
Triage
  ↓
Remediate where appropriate
  ↓
Rescan
  ↓
Verify
```

A scanner result should be investigated rather than blindly installing packages simply to make a finding disappear.

## Kubernetes and CI/CD commands practiced

```bash
# Kubernetes
kubectl get nodes
kubectl get pods
kubectl describe pod <pod>
kubectl logs <pod>
kubectl logs <pod> --previous
kubectl get deployments
kubectl rollout status deployment/system-monitor
kubectl rollout history deployment/system-monitor
kubectl rollout undo deployment/system-monitor
kubectl apply -f k8s/deployment.yaml
kubectl set image deployment/system-monitor system-monitor=system-monitor:1.1

# Docker
docker build -t system-monitor:1.1 .
docker run --rm system-monitor:1.1
docker ps
docker ps -a
docker logs <container>
docker exec -it <container> sh
docker inspect <image>
docker history <image>

# Security
trivy image --severity HIGH,CRITICAL system-monitor:1.1
```

## Architecture

```
                  +----------------+
                  |     GitLab     |
                  | Source Control |
                  +-------+--------+
                          |
                          v
                  +----------------+
                  |    Jenkins     |
                  |   Controller   |
                  +-------+--------+
                          |
                    Docker agent
                          |
              +-----------+-----------+
              |           |           |
            Python      Docker      kubectl
             tests       CLI          |
                                      |
                                      v
                              +---------------+
                              |  Kubernetes   |
                              |   Cluster     |
                              |               |
                              |  2 replicas   |
                              +---------------+
```

## Artifactory

**Artifactory has been studied as an artifact repository/private Docker registry concept, but it has not been implemented in this lab yet.**

Typical production flow:

```
Jenkins
   ↓
docker build
   ↓
docker tag
   ↓
Artifactory
   ↓
docker push
   ↓
Kubernetes pulls image
```

This project does **not** claim an Artifactory integration that has not been tested.

## Current DevSecOps Direction

The lab has now covered the core foundations:

- CI/CD
- Docker
- Kubernetes
- Jenkins agents
- Credentials handling
- Artifact traceability
- Container vulnerability scanning
- Kubernetes troubleshooting
- Basic container/Kubernetes security concepts

Potential future extensions include:

- Terraform / Infrastructure as Code
- GitOps with Argo CD or Flux
- Prometheus/Grafana monitoring
- Artifactory integration
- Kubernetes security hardening
- Additional CI/CD security gates

These will only be marked as implemented after they are actually tested.

## Learning Outcomes

This project demonstrates practical understanding of:

- CI/CD pipeline design
- Jenkins controller/agent architecture
- Docker containerization
- Kubernetes deployments and rolling updates
- Kubernetes troubleshooting
- Container security scanning
- Credential handling
- Artifact traceability
- Linux/Bash-based automation
- Basic DevSecOps workflow

## Related Work

The hands-on implementation is maintained separately in GitLab:

**Python System Monitor:**  
https://gitlab.com/personal-group3364440/python-system-monitor

This GitHub repository serves as the portfolio-facing documentation of the DevOps lab.

## Disclaimer

This is a personal hands-on learning project and is not presented as professional production experience.

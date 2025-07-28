# devops-stack

A pre-configured Docker Compose stack for running a self-hosted CI/CD pipeline with essential tools like GitLab, GitLab Runner, Prometheus, Grafana, SonarQube, Gitleaks, and PostgreSQL.

# Table of Contents

- [Overview](#overview)
- [Services Overview](#services-overview)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Environment Variables](#environment-variables)
  - [Launch the Stack](#launch-the-stack)
  - [Access Web Interfaces](#access-web-interfaces)
  - [Initial Credentials](#initial-credentials)
- [Directory Structure](#directory-structure)
- [To-Do](#to-do)

## Services Overview

> These ports can be set through changing the `.env` file.

| Service          | Description                                              | Port   |
| ---------------- | -------------------------------------------------------- | ------ |
| GitLab CE        | Git repository manager with CI/CD capabilities           | 80/443 |
| GitLab Runner    | Executes CI jobs defined in `.gitlab-ci.yml`             | -      |
| Docker-in-Docker | Builds and tests Docker images within CI jobs            | -      |
| Prometheus       | Metrics and monitoring tool                              | 9090   |
| Grafana          | Visualization tool for metrics (connected to Prometheus) | 3030   |
| SonarQube        | Code quality and security analysis                       | 9000   |
| Gitleaks         | Detect secrets and credentials in code                   | -      |
| PostgreSQL       | Backend DB for SonarQube                                 | -      |

## Getting Started

### Prerequisites

* Docker
* Docker Compose

### Environment Variables

Create a `.env` file in the root directory with the following:
> Following is a sample `.env` file
```dotenv
GITLAB_HOST=gitlab.example.com
GITLAB_EXTERNAL_URL=http://${GITLAB_HOST}
GITLAB_REGISTRY_URL=http://${GITLAB_HOST}:<REGISTRY_PORT>
GITLAB_SSH_PORT=222
GITLAB_HTTP_PORT=8088
GITLAB_HTTPS_PORT=4433
GITLAB_REGISTRY_PORT=5050
GITLAB_SSH_HOST_PORT=22

PROMETHEUS_PORT=9090

GRAFANA_PORT=3030

SONARQUBE_PORT=9000
SONARQUBE_JDBC_URL=jdbc:postgresql://db:5432/sonar
SONARQUBE_JDBC_USERNAME=sonar                              # used for postgres aswell
SONARQUBE_JDBC_PASSWORD=sonar                              # used for postgres aswell
SONARQUBE_DB_NAME=sonar                                    # used for postgres aswell

COMPOSE_PROJECT_NAME=DevSecOps-Stack
```

### Launch the Stack

```bash
docker compose up -d
```

### Access Web Interfaces

* GitLab: [http://localhost](http://localhost)
* Prometheus: [http://localhost:9090](http://localhost:9090)
* Grafana: [http://localhost:3030](http://localhost:3030)
* SonarQube: [http://localhost:9000](http://localhost:9000)

### Initial Credentials

| Service   | Username | Password                                      |
| --------- | -------- | --------------------------------------------- |
| GitLab    | root     | stored at gitlab/config/initial_root_password |
| Grafana   | admin    | admin                                         |
| SonarQube | sonar    | sonar                                         |

## Directory Structure
> The `prometeus.yml` file needs to be created after setting up the docker containers

```
.
├── docker-compose.yml
├── .env
├── gitlab/
│   ├── config/
│   ├── data/
│   └── logs/
├── runner/
│   └── config/
├── prometheus/
│   └── prometheus.yml
├── grafana/
│   ├── config/
│   ├── provisioning/
│   └── dashboards/
├── sonarqube/
│   ├── conf/
│   ├── data/
│   ├── logs/
│   └── extensions/
└── postgresql/
```

## TO-DO

* NGINX reverse proxy
* OWASP ZAP and Burp Suite integration

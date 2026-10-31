# Final Project — CI/CD Pipeline with GitHub Actions and OpenShift

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Python 3.9](https://img.shields.io/badge/Python-3.9-green.svg)](https://shields.io/)

This repository contains the CI/CD pipeline implementation and application source code for the Coursera Final Project, demonstrating automated continuous integration and continuous deployment using **GitHub Actions**, **Tekton**, and **Red Hat OpenShift**.

## Application Overview

The application is a lightweight RESTful **Flask Counter Service** providing hit-counter management endpoints (create, read, update, list, and delete counters).

### Technologies Used

- **Python & Flask**: Core application runtime and web microservice framework.
- **GitHub Actions**: Continuous integration workflow automating static code analysis and test execution on push and pull requests.
- **Flake8**: Static analysis tool for linting code quality, PEP 8 compliance, and complexity checks.
- **Nose**: Unit testing and test automation framework with coverage and spec reporting (`nosetests`).
- **Tekton**: Cloud-native CI/CD pipeline engine executing modular, reusable tasks in Kubernetes/OpenShift.
- **Buildah**: OCI-compliant container image builder running inside the cluster without requiring a Docker daemon.
- **Red Hat OpenShift**: Enterprise container orchestration platform hosting the pipeline and deployed application service.

## CI/CD Pipeline Stages

The automated OpenShift / Tekton pipeline (`oc-final`) consists of six discrete stages:

1. **Cleanup**: Safely purges the shared workspace persistent volume before subsequent tasks run.
2. **Git clone**: Clones the application source code from the Git repository branch (`main`) into the workspace.
3. **Flake8 linting**: Analyzes Python code against PEP 8 style guides and cyclomatic complexity limits.
4. **Nose unit testing**: Runs the automated test suite using `nosetests` and validates code behavior.
5. **Buildah image build**: Builds an OCI container image using the project `Dockerfile` and pushes it to the OpenShift internal image registry.
6. **OpenShift deployment**: Deploys or updates the containerized application on OpenShift exposing port 8000 and verifies rollout completion.

## License

Licensed under the Apache License, Version 2.0. See [LICENSE](LICENSE) for details.

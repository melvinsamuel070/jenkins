# Jenkins Pipeline for CI/CD Deployment

This repository contains a Jenkins pipeline used to automate the CI/CD process for building, testing, and deploying applications using Docker and Node.js. The pipeline is defined using a `Jenkinsfile`.

##  Project Structure

- **Jenkinsfile** — Defines the complete pipeline process.
- **Pipeline Stages**:
  - Checkout source code
  - Initial setup (echo/logging)
  - Docker build and push (commented)
  - Run production tasks (commented)
  - Deploy to EC2 (commented)
  - Clone repo on EC2 (commented)

---

##  Jenkins Pipeline Breakdown

### Agent
- The pipeline runs on a Jenkins agent labeled `agent`.

---

###  Stage: `Checkout`
Checks out the source code from the GitHub repository.

```groovy
git branch: 'master', 
    url: 'https://github.com/melvinsamuel070/jenkins.git'

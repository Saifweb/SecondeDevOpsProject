# DevOps Project 
![image](https://github.com/Saifweb/SecondeDevOpsProject/assets/98279240/99b41cc0-425e-4b38-9af3-b3d72e54b448)

## Overview
This DevOps project involves setting up a Kubernetes CI/CD pipeline using Jenkins, Maven, SonarQube, Trivy, and ArgoCD. The pipeline automates the build, test, and deployment processes, ensuring code quality and security throughout the development lifecycle.

## Jenkins Setup

### Step 1: Set Up Jenkins Instances
- Create two Jenkins instances: master and slave.
- Configure SSH connection between master and slave.

#### Installation (Slave Server):
- Install Docker: `sudo apt install docker.io`
- Add user to Docker group: `sudo usermod -aG docker $USER`
- Edit sshd_config file.
- Reload sshd server: `sudo service sshd reload`

#### Installation (Master):
- Edit sshd_config file.
- Reload sshd server: `sudo service sshd reload`
- Generate SSH key: `ssh-keygen`

### Step 2: Configure Jenkins Nodes
- Add Maven and Java plugins to Jenkins.
- Add Jenkins slave as a node in Jenkins master.

### Step 3: Build Jenkins Pipeline
- Implement a Jenkins pipeline with stages: Clean workspace, Checkout, Build app, Test app.

## SonarQube Integration

### Step 4: Integrate SonarQube
- Install SonarQube on a dedicated VM for resource isolation.
- Use a Postgres database with SonarQube for enhanced capabilities.
- Set up a firewall rule (TCP connection, port 9000).
- Access SonarQube from the browser with default credentials (admin/admin).

### Step 5: Integrate SonarQube with Jenkins
1. Integrate SonarQube with Jenkins and perform analysis.
   - Generate token from SonarQube.
   - Add token to Jenkins master credentials.

2. Implement quality gate using webhook.
   - Set up a webhook on SonarQube server to respond to Jenkins.

## Docker Image Management

### Step 6: Build Docker Images
- Create a Docker Hub access token.
- Build and push Docker images to Docker Hub.

### Step 7: Scan Docker Images with Trivy
- Use Trivy to scan Docker images for vulnerabilities.

### Step 8: Cleanup Docker Images
- Delete Docker images from Jenkins VM for space optimization.

## Deployment with ArgoCD

### Step 9: Set Up Deployment Instance
1. Install Minikube.
2. Install ArgoCD.

### Step 10: Create CD Pipeline
- Update deployment file with new image tag.
**Explanation**: Creating a new CD pipeline allows updating the deployment image tag, ensuring that the latest version of the application is deployed. This practice supports continuous delivery and ensures that the latest changes are reflected in the production environment.
  
### Step 11: Trigger CD Pipeline
- Trigger the CD pipeline for the first deployment.

## GitHub Repository
Find the deployment YAML files in the [GitHub repository]( https://github.com/Saifweb/deploy ).

## Definitions

- **SonarQube**: SonarQube is an open-source platform for continuous inspection of code quality. It performs static code analysis, identifies code smells, bugs, and security vulnerabilities in software projects.

- **ArgoCD**: ArgoCD is a declarative, GitOps continuous delivery tool for Kubernetes. It helps in automating the deployment of applications and ensures they are in sync with the desired state.

- **Maven**: Maven is a build automation tool used for managing the build lifecycle of a software project. It automates the build process, making it easier to compile source code, run tests, and package applications. This is crucial for achieving continuous integration.

-  **Trivy**: Trivy is a container image vulnerability scanner. Its primary role is to identify security vulnerabilities in container images, helping enhance the security of containerized applications.
## Results 
![image](https://github.com/Saifweb/SecondeDevOpsProject/assets/98279240/a2dce3b4-a917-4239-936a-9f3debc2f0af)

## Jenkins Pipeline Results
![image](https://github.com/Saifweb/SecondeDevOpsProject/assets/98279240/a9980d6d-375c-437c-8952-3386c58b9c9d)
![image](https://github.com/Saifweb/SecondeDevOpsProject/assets/98279240/86c17296-8c8c-410e-bbb3-fb87efe46ee3)

## Conclusion
Throughout this project, you'll not only gain practical skills in configuring and utilizing various DevOps tools but also develop a deeper understanding of the principles behind continuous integration, continuous delivery, and containerization. The combination of Jenkins, Maven, SonarQube, Trivy, and ArgoCD provides a comprehensive toolset for building, testing, and deploying applications in a reliable and secure manner. These skills are fundamental for any DevOps engineer or software developer involved in the modern software development lifecycle.


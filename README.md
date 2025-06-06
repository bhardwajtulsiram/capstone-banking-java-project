# Capstone Banking Java Project CI/CD Pipeline

This project demonstrates a CI/CD pipeline for a Java-based banking application using Jenkins Pipeline, Docker, Ansible and Terraform for AWS infrastructure automation. The process automates code building, testing, containerization, and deployment to EC2 instances.

---

## Prerequisites

- **Jenkins** server with required plugins (Pipeline, Docker, Ansible, Credentials, etc.)
- **SonarQube** server running and accessible, with authentication tokens/credentials configured in Jenkins
- **DockerHub** account and credentials stored in Jenkins (used for pushing images)
- **Ansible** installed and configured on Jenkins
- **Terraform** installed locally using VS Code
- **AWS EC2** Linux instances accessible via SSH and listed in the Ansible inventory
- **Java 11**, **Maven**, **Docker** installed on build agents

---

## Pipeline Flow

The CI/CD pipeline consists of the following automated stages (see `Jenkinsfile`):

1. **Provision Infrastructure with Terraform**
   - Use Terraform scripts to provision AWS EC2 instances (and optionally other resources) for deployment.

2.  **Checkout Source Code**
   - Jenkins checks out the code from GitHub.

3. **Compile Code**
   - Compiles the project using Maven

4. **Run Tests**
   - Executes all unit and integration tests: `mvn test`

5. **Static Code Analysis with SonarQube**
   - Runs SonarQube analysis for code quality, bugs, vulnerabilities, and code smells

6. **Package Application**
   - Packages the application as a JAR

7. **Build Docker Image**
   - Builds a Docker image using the `Dockerfile`:

8. **Docker Login & Push**
   - Logs in to DockerHub using Jenkins credentials
   - Pushes the image

9. **Configuration Management and Deploy with Ansible**
   - Runs `ansible-playbook.yml` to:
     - Update apt repositories
     - Install Docker on EC2 instances (if needed)
     - Start Docker service
     - Run the Docker container with the pushed image
---

## Executing the Pipeline

1. **Configure Jenkins:**
   - Create credentials for DockerHub, Ansible and SonarQube
   - Configure a pipeline job using the `Jenkinsfile` in this repo

2. **Run the Pipeline:**
   - Set up webhook for GitHub pushes

3. **Monitor Progress:**
   - Each stage will be visible in Jenkins UI
   - SonarQube analysis results and quality gate status will be visible in Jenkins and SonarQube dashboards
   - On success, the application will be deployed and running on the EC2 instances

---

## File Overviews

### Jenkinsfile

- Defines all pipeline stages
- Uses credentials for DockerHub and Ansible

### Dockerfile

- Builds a containerized Java app running on port 8099

### ansible-playbook.yml

- Installs Docker, starts the service, and deploys the built Docker image on all EC2 hosts in the inventory

---

## Troubleshooting

- **Terraform Apply Issues:** Ensure your AWS credentials are valid and IAM permissions are sufficient
- **SonarQube Analysis Fails:** Verify SonarQube server is reachable, authentication tokens are valid, and Jenkins is properly configured with SonarQube plugins
- **Docker Push Fails:** Check DockerHub credentials in Jenkins
- **Ansible SSH Issues:** Ensure Jenkins has SSH access to EC2s, and inventory file is correct
- **Ports:** Ensure port 8099 is open in the EC2 security group

---

## Credits

- Jenkins for CI/CD orchestration
- Docker for containerization
- Ansible for automated remote deployment


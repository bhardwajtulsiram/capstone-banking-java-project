# Capstone Banking Java Project CI/CD Pipeline

This project demonstrates a CI/CD pipeline for a Java-based banking application using Jenkins Pipeline, Docker, and Ansible. The process automates code building, testing, containerization, and deployment to EC2 instances.

---

## Prerequisites

- **Jenkins** server with required plugins (Pipeline, Docker, Ansible, Credentials, etc.)
- **DockerHub** account and credentials stored in Jenkins (used for pushing images)
- **Ansible** installed and configured on Jenkins
- **AWS EC2** Linux instances accessible via SSH and listed in the Ansible inventory
- **Java 11**, **Maven**, **Docker** installed on build agents

---

## Pipeline Flow

The CI/CD pipeline consists of the following automated stages (see `Jenkinsfile`):

1. **Checkout Source Code**
   - Jenkins checks out the code from GitHub.

2. **Compile Code**
   - Compiles the project using Maven

3. **Run Tests**
   - Executes all unit and integration tests: `mvn test`

4. **Quality Assurance**
   - Runs Checkstyle to ensure code quality

5. **Package Application**
   - Packages the application as a JAR

6. **Build Docker Image**
   - Builds a Docker image using the `Dockerfile`:

7. **Docker Login & Push**
   - Logs in to DockerHub using Jenkins credentials
   - Pushes the image

8. **Deploy with Ansible**
   - Runs `ansible-playbook.yml` to:
     - Update apt repositories
     - Install Docker on EC2 instances (if needed)
     - Start Docker service
     - Run the Docker container with the pushed image
---

## Executing the Pipeline

1. **Configure Jenkins:**
   - Create credentials for DockerHub and Ansible 
   - Configure a pipeline job using the `Jenkinsfile` in this repo

2. **Run the Pipeline:**
   - Set up webhook for GitHub pushes

3. **Monitor Progress:**
   - Each stage will be visible in Jenkins UI
   - On success, the application will be deployed and running on the EC2 instances

---

## File Overviews

### Jenkinsfile

- Defines all pipeline stages
- Uses credentials for DockerHub and Ansible

### Dockerfile

- Builds a containerized Java app running on port 8091

### ansible-playbook.yml

- Installs Docker, starts the service, and deploys the built Docker image on all EC2 hosts in the inventory

---

## Troubleshooting

- **Docker Push Fails:** Check DockerHub credentials in Jenkins
- **Ansible SSH Issues:** Ensure Jenkins has SSH access to EC2s, and inventory file is correct
- **Ports:** Ensure port 8091 is open in the EC2 security group

---

## Credits

- Jenkins for CI/CD orchestration
- Docker for containerization
- Ansible for automated remote deployment


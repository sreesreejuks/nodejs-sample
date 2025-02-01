# Multi-Stage Docker Build with Jenkins Pipeline
## This project demonstrates how to use multi-stage Docker builds in a Jenkins pipeline for efficient CI/CD.

### Project Overview
✅ Uses multi-stage builds to reduce Docker image size.

✅ Builds, tests, and runs a Node.js application inside Docker.

✅ Automates the process with Jenkins Pipeline.

✅ Pushes the Docker image to Docker Hub (optional).

### 1️⃣ Prerequisites
Before you start, make sure you have:

Jenkins installed with Docker support.

GitHub repository to store this project.

Docker installed on the Jenkins server.

(Optional) Docker Hub account for pushing images.
### 2️⃣ Project Structure
bash
Copy
Edit
📂 multi-stage-docker

│── 📜 index.js            # Simple Node.js server

│── 📜 package.json        # Dependencies file

│── 📜 Dockerfile          # Multi-stage Docker build file

│── 📜 Jenkinsfile         # Jenkins Pipeline script

│── 📜 README.md           # Project documentation

### 3️⃣ How to Run Locally

Step 1: Clone the Repo
```bash
git clone https://github.com/your-username/multi-stage-docker.git
```
```bash
cd multi-stage-docker
```

Step 2: Build & Run the Multi-Stage Docker Image

```bash
docker build -t multi-stage-app .
```
```bash
docker run -d -p 3000:3000 multi-stage-app
```
Step 3: Test the Application
```bash
curl http://localhost:3000
```

✅ You should see: "Hello, Multi-Stage Docker Build!"

### 4️⃣ Jenkins Pipeline Configuration
Step 1: Create a Pipeline in Jenkins

    Go to Jenkins → New Item → Pipeline

    Choose "Pipeline script from SCM"

    Enter your GitHub repo URL

    Save & Build Now 🚀

Step 2: Jenkinsfile (Pipeline Script)

    This pipeline:

    ✅ Checks out code

    ✅ Builds a multi-stage Docker image

    ✅ Runs and tests the container

    ✅ Pushes to Docker Hub (optional)

```yaml

pipeline {
    
    agent any
        environment {
        IMAGE_NAME = "multi-stage-app"
        DOCKER_HUB_USER = "your-dockerhub-username"  // Change this
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/your-repo.git'  // Change this
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME} ."
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    sh "docker run -d -p 3000:3000 ${IMAGE_NAME}"
                }
            }
        }

        stage('Test Application') {
            steps {
                script {
                    sh "curl http://localhost:3000"
                }
            }
        }

        stage('Push to Docker Hub') {
            when { expression { return env.DOCKER_HUB_USER != null } }
            steps {
                script {
                    sh "docker login -u ${DOCKER_HUB_USER} -p ${DOCKER_HUB_PASSWORD}"
                    sh "docker tag ${IMAGE_NAME} ${DOCKER_HUB_USER}/${IMAGE_NAME}:latest"
                    sh "docker push ${DOCKER_HUB_USER}/${IMAGE_NAME}:latest"
                }
            }
        }
    }

    post {
        always {
            sh "docker ps -a"
        }
    }
}

  
```


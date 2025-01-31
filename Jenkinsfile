pipeline {
    agent any
    
    environment {
        IMAGE_NAME = "multi-stage-app"
        DOCKER_HUB_USER = "sreesreejuks"  
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/sreesreejuks/nodejs-sample.git'  // Change this to your repo
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

        /*stage('Push to Docker Hub') {
            when { expression { return env.DOCKER_HUB_USER != null } }
            steps {
                script {
                    sh "docker login -u ${DOCKER_HUB_USER} -p ${DOCKER_HUB_PASSWORD}"
                    sh "docker tag ${IMAGE_NAME} ${DOCKER_HUB_USER}/${IMAGE_NAME}:latest"
                    sh "docker push ${DOCKER_HUB_USER}/${IMAGE_NAME}:latest"
                }
            }
        }*/
    }

    post {
        always {
            sh "docker ps -a"
        }
    }
}

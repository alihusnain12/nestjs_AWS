pipeline {
    agent any
    environment {
        CONTAINER_NAME = 'nest-app'
        IMAGE_NAME = 'nest-app_image'
        EMAIL = 'alihusnain43447@gmail.com'
        PORT = '3000'
    }
    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning repository...'
                git branch: 'main', url: 'https://github.com/alihusnain12/nestjs_AWS.git'
            }
        }
        stage("Build Docker Image") {
            steps {
                echo 'Building Docker image...'
                sh "docker build -t ${IMAGE_NAME} ."  // ✅ No sudo
            }
        }
        stage("Stop and Remove Previous Container") {
            steps {
                sh """
                echo 'Stopping and removing previous container...'
                docker stop ${CONTAINER_NAME} || true   // ✅ No sudo
                docker rm ${CONTAINER_NAME} || true     // ✅ No sudo
                """
            }
        }
        stage("Run Docker Container") {
            steps {
                sh """
                echo 'Running Docker container...'
                docker run -d --name ${CONTAINER_NAME} -p ${PORT}:${PORT} ${IMAGE_NAME}  // ✅ No sudo
                """
            }
        }
        stage("Send Email Notification") {
            steps {
                emailext(        // ✅ Fixed typo: emailtext → emailext
                    subject: "Deployment Successful",
                    body: "Deployment successful! Application is running on port ${PORT}",
                    to: "${EMAIL}"
                )
            }
        }
    }
    post {                       // ✅ Better than a stage for notifications
        failure {
            emailext(
                subject: "Deployment FAILED",
                body: "Pipeline failed. Check Jenkins logs for details.",
                to: "${EMAIL}"
            )
        }
    }
}
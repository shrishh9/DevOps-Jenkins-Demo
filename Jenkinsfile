pipeline {
    agent any
    environment {
        IMAGE = "hello-devops:${env.BUILD_NUMBER}"
        CONTAINER_NAME = "hello-devops-${env.BUILD_NUMBER}"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
                sh 'ls -la'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE} ."
            }
        }
        stage('Run Container') {
            steps {
                // stop & remove any existing container with same name (safe)
                sh "docker rm -f ${CONTAINER_NAME} || true"
                // run detached, map port 5000
                sh "docker run -d --name ${CONTAINER_NAME} -p 5000:5000 ${IMAGE}"
                // wait a second for app to start
                sh "sleep 2"
            }
        }
        stage('Smoke Test') {
            steps {
                // show the HTTP response - this will appear in Jenkins console
                sh "curl -v --retry 5 --retry-delay 1 http://localhost:5000 || true"
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: 'Dockerfile,app.py,requirements.txt,Jenkinsfile', allowEmptyArchive: true
            // Optionally keep container running. If you want to stop it at end, uncomment next line:
            // sh "docker rm -f ${CONTAINER_NAME} || true"
        }
    }
}

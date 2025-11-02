pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t hello-devops .'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    sh 'docker run -d -p 5000:5000 --name devops-container hello-devops'
                }
            }
        }
    }

    post {
        always {
            echo "Cleaning up containers..."
            sh 'docker ps -a'
        }
    }
}


pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    bat 'docker build -t hello-devops .'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    bat 'docker run -d -p 5000:5000 --name devops-container hello-devops'
                }
            }
        }
    }

    post {
        always {
            echo "Cleaning up containers..."
            bat 'docker ps -a'
        }
    }
}


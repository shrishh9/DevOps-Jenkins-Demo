pipeline {
  agent any

  environment {
    IMAGE = "hello-devops:1.0"
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          echo "🚀 Building Docker image..."
          sh "docker build -t ${IMAGE} ."
        }
      }
    }

    stage('Run Container') {
      steps {
        script {
          echo "🏃 Running container..."
          // Stop and remove any existing container (ignore if not found)
          sh "docker rm -f hello-devops || true"
          
          // Run new container
          sh "docker run -d --name hello-devops -p 5000:5000 ${IMAGE}"

          // Wait a few seconds for Flask to start
          sh "sleep 8"

          // Log output instead of health check
          echo "✅ Container started successfully!"
          sh "docker ps"
          sh "docker logs hello-devops"
        }
      }
    }
  }

  post {
    success {
      echo "✅ Build and run successful!"
    }
    failure {
      echo "❌ Build or run failed! Check logs above."
    }
  }
}

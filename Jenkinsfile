pipeline {
  agent any
  tools {
    maven 'M2_HOME'
  }
  environment {
    registry = "myratam/jenkins" // Docker image name
    registryCredential = "jenkins" // Docker registry credentials stored in Jenkins credentials manager
    gitRepo = "https://github.com/myratam/KTECH-2023.git" // Replace with your actual GitHub repository URL
    dockerfileDir = "./" // Specify the directory of your Dockerfile relative to your GitHub repo
  }
  stages {
    stage('Checkout') {
      steps {
        // Checkout the code from GitHub repository
        git branch: 'main', url: "${gitRepo}"
      }
    }

    stage('Build') {
      steps {
        // Clean, compile, and package your application
        sh 'mvn clean'
        sh 'mvn install'
        sh 'mvn package'
      }
    }

    stage('Test') {
      steps {
        // Run tests
        echo "Running tests..."
        sh 'mvn test'
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          // Specify Dockerfile directory if it's inside a folder (e.g., 'docker/')
          docker.build("${registry}:${BUILD_NUMBER}", "${dockerfileDir}")
        }
      }
    }

    stage('Push to Docker Hub') {
      steps {
        script {
          // Log in to Docker Hub and push the image
          docker.withRegistry('https://index.docker.io/v1/', registryCredential) {
            docker.image("${registry}:${BUILD_NUMBER}").push()
          }
        }
      }
    }
  }
  post {
    always {
      // Clean up Docker images after pipeline execution
      sh 'docker system prune -f'
    }
  }
}

pipeline {
  agent any
  tools {
     maven 'M2_HOME'
  }
  environment {
     registry = "myratam/jenkins" // Docker image name
     registryCredential = "jenkins" // Docker registry credentials
     gitRepo = "https://github.com/yourusername/yourrepo.git" // Replace with your actual GitHub repository URL
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
        sh 'mvn clean'
        sh 'mvn install'
        sh 'mvn package'
      }
    }
   
    stage('Test') {
      steps {
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
          docker.withRegistry('https://index.docker.io/v1/', registryCredential) {
            docker.image("${registry}:${BUILD_NUMBER}").push()
          }
        }
      }
    }
  }
}

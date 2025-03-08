pipeline {
  agent any
  tools {
     maven 'M2_HOME'
  }
  environment {
     registry = "myratam/jenkins"
     registryCredential = "jenkins"
  }
  stages {
    stage('Build') {
      steps {
       sh 'mvn clean install package'
     }
   }
    stage('Test') {
      steps {
       echo "Running tests"
       sh 'mvn test'
     }
   }
    stage('Deploy') {
      steps {
        script {
          // Authenticate with Docker Hub using Jenkins credentials
          withDockerRegistry([credentialsId: registryCredential, url: ""]) {
            def image = docker.build("${registry}:$BUILD_NUMBER")
            image.push() // Push the built image to Docker Hub
            image.push("latest") // Update the "latest" tag
          }
        }
      }
    }
  }
}

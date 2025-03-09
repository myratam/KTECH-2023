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
          docker.build("${registry}:${BUILD_NUMBER}")
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

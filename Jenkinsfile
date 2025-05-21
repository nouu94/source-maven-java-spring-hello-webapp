pipeline {
  agent any
  tools {
    maven 'Maven-3.9.6'
  }
  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', 
        url: 'https://github.com/nouu94/source-maven-java-spring-hello-webapp.git'
      }
    }

    stage('Build') {
      steps {
        sh 'mvn clean package -DskipTests=true'
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          docker.build("nouu94/myhello:${env.BUILD_NUMBER}")
	  docker.build("nouu94/myhello:latest")
        }
      }
    }

    stage('Publish Docker Image') {
      steps {
        withDockerRegistry(
          credentialsId: 'docker-registry-credential', 
          url: 'https://index.docker.io/v1/'
        ) {
          sh "docker push nouu94/myhello:${env.BUILD_NUMBER}"
          sh "docker push nouu94/myhello:latest"
        }
      }
    }
  }
}

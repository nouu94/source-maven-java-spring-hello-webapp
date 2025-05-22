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

        stage('Update Kubernetes Manifests file') {
            steps {
                dir('k8s-manifests') {
                    git branch: 'master',
                         url: 'https://github.com/nouu94/mykube-resource2.git',
		
		script {
		  withCredentials([usernamePassword(
		  credentialsId: 'k8s-manifests-update-token',
		    usernameVariable: 'GIT_USER',
		    passwordVariable: 'GIT_TOKEN'
		  )]) {

		    sh "sed -i 's|image: nouu94/myhello:.*|image: nouu94/myhello:${env.BUILD_NUMBER}|' deployment.yaml"

		    sh 'git config user.name "nouu94"'
		    sh 'git config user.email "nouu30133@naver.com"'

		    
		    sh 'git remote set-url origin https://${GIT_USER}:${GIT_TOKEN}@github.com/nouu94/mykube-resource2.git'

		    sh 'git add .'
		    sh "git commit -m '[CI] Update image tag to ${env.BUILD_NUMBER}' || echo 'Nothing to commit'"
		    sh 'git push origin master'
		}
	    }
                }
            }
        }
    }
}

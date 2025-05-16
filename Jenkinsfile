pipeline {
    agent { label 'jenkins-node' }

    triggers {
      pollSCM ('H/5 * * * *')
}

    stages{
        stage ('Checkout'){ 
            steps {
                git branch : 'main',
		url : 'https://github.com/nouu94/source-maven-java-spring-hello-webapp.git'
            }
        }

        stage ('Build') { 
            steps {
              sh 'mvn clean package'
            }
        }

        stage ('Deploy') { 
            steps {
	      deploy adapters: [tomcat9(credentialsId: 'tomcat-manager', url: 'http://192.168.56.102:8080')], contextPath: null, war: 'target/hello-world.war'
            }
        }
    }
}

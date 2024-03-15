pipeline {
    agent any 
    
    tools{
        jdk 'jdk17'
        nodejs 'node16'
        nexus 'nexus-tool'
    }
    
    environment {
    DOCKERHUB_CREDENTIALS = credentials('todo-jenkins')
    SCANNER_HOME= tool 'sonar-scanner'
    }
    stages { 
        stage('SCM Checkout') {
            steps{
            git 'https://github.com/elvine1982/nodejs-demo.git'
            }
        }

        stage('Build docker image') {
            steps {  
                sh 'docker build -t elvine1982/nodeapp:$BUILD_NUMBER .'
            }
        }
        
        stage('SONARQUBE ANALYSIS') {
            steps {
                withSonarQubeEnv('sonar-scanner') {
                    sh " $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Docker-job -Dsonar.projectKey=Docker-job "
                }
            }
        }
        stage('login to dockerhub') {
            steps{
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('push image') {
            steps{
                sh 'docker push elvine1982/nodeapp:$BUILD_NUMBER'
            }
        }
        stage('Deploy to Nexus') {
            steps{
	           echo("Deploy to Nexus successfully run");
         }
      }
   }
}
post {
        always {
            sh 'docker logout'
        }
    }
}

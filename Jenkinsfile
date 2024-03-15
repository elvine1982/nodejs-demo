pipeline {
    agent any 
    environment {
    DOCKERHUB_CREDENTIALS = credentials('todo-jenkins')
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
}
post {
        always {
            sh 'docker logout'
        }
    }
}


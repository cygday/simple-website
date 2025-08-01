pipeline {
    agent any
    stages {
        stage('checkout') {
            steps {
                git 'https://github.com/cygday/simple-website.git'
            }
        }
        stage('build docker image') {
            steps {
                sh 'docker build -t cygday/simple-website:latest .'
            }
        }
        stage('push to docker hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'cygday', passwordVariable: 'cygday1@_Aa')]) {
                    sh '''echo  "$PASSWORD" | docker login -u "cygday" --password-stdin docker push cygday/simple-website:latest'''
                }
            }
        }
        stage('deploy to kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
            }
        }
    }
}
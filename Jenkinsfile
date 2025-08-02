pipeline {
    agent any

    environment {
        IMAGE_NAME = "cygday/simple-website:latest"
    }
    
    stages {
        stage("checkout") {
            steps {
                // use build-in scm checkout
                sh "echo checkout scm"
            }
        }
        stage("build docker image") {
            steps {
                sh "docker build -t $IMAGE_NAME ."
            }
        }
        stage("push to docker hub") {
            steps {
                withCredentials([usernamePassword(credentialsId: "dockerhub", usernameVariable: "DOCKER_USER", passwordVariable: "DOCKER_PASS")]) {
                    sh """echo  "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin 
                          docker push $IMAGE_NAME"""
                }
            }
        }
        stage("deploy to kubernetes") {
            steps {
                sh "kubectl apply -f k8s/deployment.yaml"
            }
        }
    }
}

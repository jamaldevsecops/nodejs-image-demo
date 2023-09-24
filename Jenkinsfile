def dockerImage

pipeline {
    agent {
        label 'RND-SERVER-03'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Stop and Remove Previous Container') {
            steps {
                script {
                    def previousContainerName = 'my-node-app'
                    def isContainerRunning = sh(script: "docker ps -q -f name=${previousContainerName}", returnStatus: true) == 0
                    
                    if (isContainerRunning) {
                        sh "docker stop ${previousContainerName}"
                        sh "docker rm ${previousContainerName}"
                    }
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = 'my-node-app'
                    sh "docker build -t ${dockerImage} -f /path/to/your/Dockerfile ."
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                script {
                    def containerName = 'my-node-app'
                    sh "docker run --name ${containerName} -p 8080:8080 -d ${dockerImage}"
                }
            }
        }
    }
    post {
        success {
            echo "Deployment successful!"
        }
        failure {
            echo "Deployment failed!"
        }
    }
}

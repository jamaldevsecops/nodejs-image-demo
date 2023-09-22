pipeline {
    agent RND-SERVER-03
    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the Git repository
                checkout scm
            }
        }
        stage('Build Docker Image') {
            steps {
                // Build a Docker image from the Dockerfile in the repository
                script {
                    def imageName = 'node-demo'
                    def dockerFile = 'Dockerfile' // Assumes Dockerfile is in the repository root
                    
                    sh "docker build -t ${imageName} -f ${dockerFile} ."
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                // Run a Docker container from the built image
                script {
                    def containerName = 'node-demo'
                    def portMapping = '8080:8080'
                    def imageName = 'node-demo'
                    
                    sh "docker run --name ${containerName} -p ${portMapping} -d ${imageName}"
                }
            }
        }
    }
    post {
        success {
            // Add post-build actions if needed
        }
        failure {
            // Handle failures if needed
        }
    }
}

def dockerImage

pipeline {
    agent {
        label 'RND-SERVER-03'
    }
    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from your Git repository
                checkout scm
            }
        }
        stage('Stop and Remove Docker Container') {
            steps {
                script {
                    // Define the Docker container name
                    def containerName = 'my-node-app'
                    
                    // Check if the container is running
                    def isRunning = sh(script: "docker inspect -f '{{.State.Running}}' ${containerName}", returnStatus: true) == 0
                    
                    if (isRunning) {
                        // Stop the existing container if it's running
                        sh "docker stop ${containerName}"
                        
                        // Remove the existing container
                        sh "docker rm ${containerName}"
                        
                        echo "Stopped and removed existing container: ${containerName}"
                    } else {
                        echo "Container ${containerName} is not running, no need to stop or remove."
                    }
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Define the Docker image name
                    dockerImage = 'my-node-app'
                    // Build the Docker image using the Dockerfile from the project directory
                    sh "docker build -t ${dockerImage} -f Dockerfile ."
                    // Replace "/path/to/your/Dockerfile" with the actual path to your Dockerfile
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                script {
                    // Define the Docker container name
                    def containerName = 'my-node-app'
                    // Run the Docker container
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

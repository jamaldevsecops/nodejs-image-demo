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
        stage('Stop and Remove Previous Container') {
            steps {
                script {
                    // Define the Docker container name for the previous instance
                    def previousContainerName = 'my-node-app'
                    
                    // Check if a container with the same name exists (running or stopped)
                    def containerExists = sh(script: "docker ps -aq -f name=${previousContainerName}", returnStatus: true) == 0
                    
                    if (containerExists) {
                        // Stop and remove the previous container
                        sh "docker stop ${previousContainerName}"
                        sh "docker rm ${previousContainerName}"
                        echo "Stopped and removed the previous container ${previousContainerName}"
                    } else {
                        echo "No container with the name ${previousContainerName} found."
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

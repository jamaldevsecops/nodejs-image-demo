def dockerImage

pipeline {
    agent {
        label 'RND-SERVER-03'
    }
    stages {
        stage('Stop and Remove Previous Container') {
            steps {
                script {
                    // Define the Docker container name for the previous instance
                    def previousContainerName = 'my-node-app'
                    
                    // Check if a container with the same name is running
                    def isContainerRunning = sh(script: "docker ps -q -f name=${previousContainerName}", returnStatus: true) == 0
                    
                    if (isContainerRunning) {
                        // Stop and remove the previous container if it's running
                        sh "docker stop ${previousContainerName}"
                        sh "docker rm ${previousContainerName}"
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

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
        
        stage('Build Docker Image') {
            steps {
                script {
                    // Define the Docker image name
                    def dockerImage = 'my-node-app'

                    // Build the Docker image using the provided Dockerfile
                    sh "echo 'FROM node:10-alpine\n\
                        RUN mkdir -p /home/node/app/node_modules && chown -R node:node /home/node/app\n\
                        WORKDIR /home/node/app\n\
                        COPY package*.json ./\n\
                        USER node\n\
                        RUN npm install\n\
                        COPY --chown=node:node . .\n\
                        EXPOSE 8080\n\
                        CMD [ \"node\", \"app.js\" ]' > Dockerfile"
                    sh "docker build -t ${dockerImage} ."
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

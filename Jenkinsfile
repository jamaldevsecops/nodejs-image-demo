pipeline {
    agent RND-SERVER-03

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from your Git repository
                checkout scm
            }
        }
        
        stage('Build Docker Image') {
            steps {
                // Define the Docker image name
                def dockerImage = 'my-node-app'

                // Build the Docker image using the provided Dockerfile
                script {
                    def dockerfile = '''
                        FROM node:10-alpine

                        RUN mkdir -p /home/node/app/node_modules && chown -R node:node /home/node/app

                        WORKDIR /home/node/app

                        COPY package*.json ./

                        USER node

                        RUN npm install

                        COPY --chown=node:node . .

                        EXPOSE 8080

                        CMD [ "node", "app.js" ]
                    '''
                    sh "echo '${dockerfile}' > Dockerfile"
                    sh "docker build -t ${dockerImage} ."
                }
            }
        }
        
        stage('Run Docker Container') {
            steps {
                // Define the Docker container name
                def containerName = 'my-node-app'

                // Run the Docker container
                sh "docker run --name ${containerName} -p 8080:8080 -d ${dockerImage}"
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

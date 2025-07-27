pipeline{
    agent any

    environment {
        IMAGE_NAME = 'my-docker-jenkins-app'
        DOCKERHUB_USER = 'your_dockerhub_username'
        DOCKERHUB_CREDENTIALS_ID = 'docker-hub-credentials-id'
    }

    stages{
        stage('Clone Repository'){
            steps{
                git 'https://github.com/yourusername/my-docker-app.git'
            }
        }

        stage('Build Docker Container'){
            steps{
                script {
                    dockerImage.run("-d -p 5000:5000")
                }
            }
        }

        stage('Push to DockerHub'){
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/' , "${DOCKERHUB_CREDENTIALS_ID}") {
                    // dockerImage.push("latest")}
                }
            }
        }
    }

    post {
    always {
        echo "Cleaning up resources..."
    }
    success {
        echo "Pipeline executed successfully!"
    }
    failure {
        echo "Pipeline failed!"
    }
}
}


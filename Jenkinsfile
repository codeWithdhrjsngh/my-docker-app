pipeline{
    agent any

    environment {
        IMAGE_NAME = 'myDockerApp'
        DOCKERHUB_USER = 'imdhrjsngh'
    }

    stages{
        stage('Clone Repository'){
            steps{
                git 'https://github.com/codeWithdhrjsngh/my-docker-app.git'
            }
        }

        stage('Build Docker Image'){
            steps{
                script {
                    dockerImage = docker.build("${IMAGE_NAME}")
                }
            }
        }

        stage('Run Container'){
            steps{
                script {
                    dockerImage.run("-d -p 5000:5000")
                }
            }
        }

        stage('Push to DockerHub'){
            when {
                expression { return env.DOCKERHUB_USER != null }
            }
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/' , 'dockerHub-credentials-id') {
                     dockerImage.push("latest")
                    }
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


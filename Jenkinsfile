pipeline{
    agent any {
        docker {
            image 'docker:20.10'   // official Docker CLI image
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    
    environment {
        IMAGE_NAME = 'myDockerApp'
    }

    stages{
        stage('Clone Repository'){
            steps{
                git branch: 'main', url: 'https://github.com/codeWithdhrjsngh/my-docker-app.git', credentialsId: 'github-credentials'

            }
        }

        stage('Build Docker Image') {
        steps {
            sh 'docker build -t ${IMAGE_NAME}:latest .'
             }
        }


        stage('Run Container') {
            steps {
                sh 'docker run -d -p 5000:5000 --name ${IMAGE_NAME}_container ${IMAGE_NAME}:latest'
            }
        }

        stage('Push to DockerHub'){
            when {
                expression { return env.DOCKERHUB_USER != null }
            }
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/' , 'dockerhub-credentials') {
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






pipeline {
    agent {
        docker {
            image 'docker:20.10'   // Docker CLI image
            args '-v /var/run/docker.sock:/var/run/docker.sock'  // give access to host Docker
        }
    }

    environment {
        IMAGE_NAME = 'myDockerApp'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/codeWithdhrjsngh/my-docker-app.git', credentialsId: 'github-credentials'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME} .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d -p 5000:5000 --name ${IMAGE_NAME}_container ${IMAGE_NAME}'
            }
        }

        stage('Push to DockerHub') {
            when {
                expression { return env.DOCKERHUB_USER != null }
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                      echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                      docker tag ${IMAGE_NAME}:latest $DOCKER_USER/${IMAGE_NAME}:latest
                      docker push $DOCKER_USER/${IMAGE_NAME}:latest
                    '''
                }
            }
        }
    }

    post {
        always {
            echo "Cleaning up resources..."
            sh 'docker ps -a'
        }
        success {
            echo "Pipeline executed successfully!"
        }
        failure {
            echo "Pipeline failed!"
        }
    }
}

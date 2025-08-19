pipeline{
  //  agent any
    agent {
        docker {
            image 'docker:20.10'    // official docker CLI image
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    
    environment {
        IMAGE_NAME = 'myDockerApp'
        CONTAINER_NAME = 'myDockerApp_container'
    }

    stages{
        stage('Clone Repository'){
            steps{
                git branch: 'main', url: 'https://github.com/codeWithdhrjsngh/my-docker-app.git', credentialsId: 'github-credentials'

            }
        }

       /* stage('Build Docker Image'){
            steps{
                script {
                    dockerImage = docker.build("${IMAGE_NAME}")
                }
            }
        }
      */

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:latest .'
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




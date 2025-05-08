pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'sriramakula212/my-node-app'
        DOCKER_CREDENTIALS_ID = 'dockerhub-creds'  // Jenkins credential ID
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/SriramAkula/my-node-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${DOCKER_IMAGE}:${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
                        dockerImage.push()
                        dockerImage.push('latest')
                    }
                }
            }
        }

        stage('Clean Up') {
            steps {
                bat "docker rmi ${DOCKER_IMAGE}:${env.BUILD_NUMBER} || exit 0"
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
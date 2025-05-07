pipeline {
    agent any

    environment {
        FRONTEND_IMAGE = 'nandhita22cse126/client-app'
        BACKEND_IMAGE = 'nandhita22cse126/server-app'
        DOCKER_CREDENTIALS_ID = 'Docker_cred'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', credentialsId: 'github_cred', url: 'https://github.com/nandhu6804/password_manager.git'
            }
        }

        stage('Build Frontend & Backend Images') {
            steps {
                script {
                    docker.build(FRONTEND_IMAGE, '--no-cache ./client')
                    docker.build(BACKEND_IMAGE, '--no-cache ./server')
                }
            }
        }

        stage('Push Images to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS_ID) {
                        docker.image(FRONTEND_IMAGE).push('latest')
                        docker.image(BACKEND_IMAGE).push('latest')
                    }
                }
            }
        }

        stage('Cleanup') {
            steps {
                sh 'docker system prune -f'
            }
        }
    }

    post {
        success {
            echo '🎉 client and server pushed to Docker Hub!'
        }
        failure {
            echo '❌ Something went wrong with the build or push.'
        }
    }
}
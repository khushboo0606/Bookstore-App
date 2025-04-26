pipeline {
    agent any

    environment {
        DOTNET_VERSION = '8.0'
        DOCKER_IMAGE = 'bookstore-app'
        DOCKER_CONTAINER = 'bookstore-container'
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/khushboo0606/Bookstore-App.git'
            }
        }

        stage('Restore Dependencies') {
            steps {
                bat 'dotnet restore'
            }
        }

        stage('Build Project') {
            steps {
                bat 'dotnet build --configuration Release'
            }
        }

        stage('Publish Project') {
            steps {
                bat 'dotnet publish -c Release -o publish'
            }
        }

        stage('Docker Cleanup') {
            steps {
                script {
                    bat """
                    docker stop %DOCKER_CONTAINER% || echo "No running container to stop"
                    docker rm %DOCKER_CONTAINER% || echo "No container to remove"
                    """
                }
            }
        }

        stage('Docker Build') {
            steps {
                bat 'docker build -t %DOCKER_IMAGE% .'
            }
        }

        stage('Docker Run') {
            steps {
                bat 'docker run -d -p 5000:80 --name %DOCKER_CONTAINER% %DOCKER_IMAGE%'
            }
        }
    }

    post {
        always {
            echo '✅ CI/CD Pipeline Finished!'
        }
    }
}

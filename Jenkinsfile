pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'bookstorewebapp'
        DOCKER_CONTAINER = 'bookstore-container'
    }

    stages {
        stage('Clone Repo') {
            steps {
                git url: 'https://github.com/khushboo0606/Bookstore-App.git'
            }
        }

        stage('Build Project') {
            steps {
                sh 'dotnet build'
            }
        }

        stage('Publish Project') {
            steps {
                sh 'dotnet publish -c Release -o out'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Docker Run') {
            steps {
                script {
                    sh 'docker stop $DOCKER_CONTAINER || true'
                    sh 'docker rm $DOCKER_CONTAINER || true'
                    sh 'docker run -d -p 5000:80 --name $DOCKER_CONTAINER $DOCKER_IMAGE'
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished!'
        }
    }
}

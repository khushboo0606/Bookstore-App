pipeline {
    agent any

    environment {
        DOTNET_VERSION = '8.0'
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

        stage('Docker Build') {
            steps {
                bat 'docker build -t bookstore-app .'
            }
        }

        stage('Docker Run') {
            steps {
                bat 'docker run -d -p 5000:80 --name bookstore-container bookstore-app'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished!'
        }
    }
}

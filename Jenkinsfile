pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Build stage'
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                echo 'Test stage'
                sh 'npm test || true'
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Docker build'
                sh 'docker build -t react-app .'
            }
        }

        stage('Docker Push') {
            steps {
                echo 'Docker push'
                sh 'echo pushing image'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploy stage'
            }
        }
    }
}

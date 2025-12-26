pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t myreactapp .'
            }
        }

        stage('Docker Push') {
            steps {
                sh 'docker push myreactapp'
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker run -d -p 80:80 myreactapp'
            }
        }
    }
}

pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Build started'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t nginx-test .'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                  docker rm -f nginx-test || true
                  docker run -d -p 80:80 --name nginx-test nginx-test
                '''
            }
        }
    }
}

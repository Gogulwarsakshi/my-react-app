pipeline {
    agent any

    environment {
        IMAGE_NAME = "sakshigogul/my-react-app"
        CONTAINER_NAME = "my-react"
    }

    stages {

        stage('Build') {
            steps {
                echo 'Building React app'
                sh 'npm install'
                sh 'npm run build'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests'
                sh 'npm test -- --watch=false || true'
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building Docker image'
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Push Image') {
            steps {
                echo 'Pushing image to DockerHub'
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
                    sh 'docker push $IMAGE_NAME:latest'
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying container on EC2'
                sh '''
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true
                docker run -d --name $CONTAINER_NAME -p 80:80 $IMAGE_NAME:latest
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully'
        }
        failure {
            echo '❌ Pipeline failed'
        }
    }
}













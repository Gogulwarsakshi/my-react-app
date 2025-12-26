pipeline {
    agent any

    environment {
        IMAGE_NAME = "sakshigogul/my-react-app"
        CONTAINER_NAME = "my-react-app"
    }

    stages {

        stage('Build') {
            steps {
                echo 'Building application'
                sh '''
                  npm install
                  npm run build
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests'
                sh '''
                  npm test -- --watch=false || true
                '''
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building Docker image'
                sh '''
                  docker build -t $IMAGE_NAME:latest .
                '''
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
                    sh '''
                      echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                      docker push $IMAGE_NAME:latest
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying container'
                sh '''
                  docker rm -f $CONTAINER_NAME || true
                  docker run -d -p 80:80 --name $CONTAINER_NAME $IMAGE_NAME:latest
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully 🎉'
        }
        failure {
            echo 'Pipeline failed ❌'
        }
    }
}

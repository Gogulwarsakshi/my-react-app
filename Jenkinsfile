pipeline {
    agent any

    options {
        timestamps()
    }

    environment {
        IMAGE_NAME     = "sakshigogul/my-react-app"
        CONTAINER_NAME = "my-react-app"
        APP_PORT       = "80"
        SONAR_TOKEN    = credentials('sonar-token')
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Installing dependencies & building app'
                sh '''
                ls -la
                node -v
                npm -v
                npm install
                npm run build
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests'
                sh 'npm test -- --watch=false || true'
            }
        }

        stage('SonarQube Scan') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh '''
                    sonar-scanner \
                      -Dsonar.projectKey=my-react-app \
                      -Dsonar.projectName=my-react-app \
                      -Dsonar.sources=src \
                      -Dsonar.host.url=http://54.253.20.211:9000 \
                      -Dsonar.login=$SONAR_TOKEN
                    '''
                }
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Push Image') {
            steps {
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
                sh '''
                docker rm -f $CONTAINER_NAME || true
                docker run -d -p $APP_PORT:80 --name $CONTAINER_NAME $IMAGE_NAME:latest
                '''
            }
        }
    }

    post {
        failure {
            echo '❌ Pipeline failed'
        }
        success {
            echo '✅ Pipeline completed successfully'
        }
    }
}

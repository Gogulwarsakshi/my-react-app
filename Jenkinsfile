stages {

    stage('Build') {
        steps {
            sh 'npm install'
            sh 'npm run build'
        }
    }

    stage('Test') {
        steps {
            sh 'npm test -- --watch=false || true'
        }
    }

    stage('Docker Build') {
        steps {
            sh 'docker build -t my-react-app:latest .'
        }
    }

    stage('Push Image') {
        steps {
            sh 'docker push my-react-app:latest'
        }
    }

    stage('Deploy') {
        steps {
            sh '''
            docker stop react-app || true
            docker rm react-app || true
            docker run -d --name react-app -p 3000:80 my-react-app:latest
            '''
        }
    }
}










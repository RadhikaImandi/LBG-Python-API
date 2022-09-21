pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh '''
                docker build -t eesh12345/automated-image:latest -t eesh12345/automated-image:$BUILD_NUMBER .
                docker push eesh12345/automated-image:latest
                docker push eesh12345/automated-image:$BUILD_NUMBER
                '''
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                ssh -i '~/.ssh/id_rsa' jenkins@34.142.33.108 << EOF
                docker stop auto-container
                docker rm auto-container
                docker run -d -p 8080:8080 --name auto-container eesh12345/automated-image:latest
                '''
            }
        }
    }
}
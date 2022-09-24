pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh '''
                docker build -t gcr.io/lbg7-20220905/automated-image:latest -t gcr.io/lbg7-20220905/automated-image:$BUILD_NUMBER .
                docker push gcr.io/lbg7-20220905/automated-image:latest
                docker push gcr.io/lbg7-20220905/automated-image:$BUILD_NUMBER
                '''
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                ssh -i '~/.ssh/id_rsa' jenkins@34.175.165.167 << EOF
                docker stop auto-container
                docker rm auto-container
                docker run -d -p 8080:8080 --name auto-container gcr.io/lbg7-20220905/automated-image:latest
                '''
            }
        }
    }
}
pipeline{
    agent any
    environment {
        NODE_VERSION = '24.4.1'
    }
    stages{
        stage('nodejs-24.4.1'){
            steps {
                sh '''
                    node -v
                    npm -v
                '''
            }
        }
    }
}
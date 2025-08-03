pipeline{
    agent any
    tools {
        nodejs 'nodejs-24.4.1'
    }
    stages{
        stage('VM Node Version'){
            steps {
                sh '''
                    node -v
                    npm -v
                '''
            }
        }
    }
}
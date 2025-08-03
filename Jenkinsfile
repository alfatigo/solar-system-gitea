pipeline {
    agent any
    tools {
        nodejs 'nodejs-24.4.1'
    }
    stages {
        stage('Installing Dependencies') {
            steps {
                sh 'npm install --no-audit'
            }
        }
        stage('Dependency Scanning'){
            parallel {                            
                stage('NPM Dependency Audit') {
                    steps {
                        sh '''
                            npm audit --audit-level=critical
                            echo $?
                        '''            
                    }
                }
                stage('Owasp Dependency Check') {
                    steps {
                        dependencyCheck additionalArguments: '''
                        --scan \'./\'
                        --out \'./\'
                        --format \'ALL\'                    
                        --prettyPrint''', odcInstallation: 'Owasp-DepCheck-10'
                    }       
                }
            }
        }
    }
}

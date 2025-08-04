pipeline {
    agent any
    tools {
        nodejs 'nodejs-24.4.1'
    }
    environment {
        MONGO_URI = "mongodb+srv://superdata.wlgwurn.mongodb.net/superData"
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

                        dependencyCheckPublisher failedTotalCritical: 1, pattern: 'dependency-check-report', skipNoReportFiles: true

                        publishHTML([allowMissing: true, alwaysLinkToLastBuild: true, icon: '', keepAll: true, reportDir: './', reportFiles: 'dependency-check-report.xml', reportName: 'Dependency Check HTML Report', reportTitles: '', useWrapperFileDirectly: true])

                        junit allowEmptyResults: true, keepProperties: true, testResults: 'dependency-check-junit.xml'
                    }       
                }
            }
        }
        stage('Unit Testing') {
            steps {

                withCredentials([usernamePassword(credentialsId: 'mongo-db-credentials', passwordVariable: 'MONGO_PASSWORD', usernameVariable: 'MONGO_USERNAME')]) {
                        sh 'npm test'
                    }

                junit allowEmptyResults: true, keepProperties: true, testResults: 'test-results.xml'
                 
            }
        }
    }
}

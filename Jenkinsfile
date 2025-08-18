pipeline {
    agent {
        node {
        label 'AGENT-1'
        }
    }
    environment {
        appVersion = ''
        REGION = 'us-east-1'
        ACC_ID = '025523569021'
        PROJECT = 'roboshop'
        COMPONENT = 'catalogue'
    }
    options {
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
    }

    stages{ 
        stage('Read Package Json') {
            steps {
                script {
                    def packageJson = readJSON file : 'package.json'
                    appVersion = packageJson.version
                    echo "Package version : ${appVersion}"
                }
            }
        }
        stage('Installing Dependencies') {
            steps {
                script {
                    sh """
                        npm install
                    """
                }
            }
        }
        stage('Docker Build') {
            steps {
                script {
                    withAWS(credentials: 'aws-creds', region: 'us-east-1}') {
                        sh """
                            aws ecr get-login-password --region ${REGION} | docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com
                            docker build -t ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion}
                            docker push ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion}
                        """
                }
            }
        }        
    }

    post {
        always {
            echo 'I will always says Hello again!'
            deleteDir()
        }
        success {
            echo 'Hello Success'
        }
        failure {
            echo 'Hello Failure'
        }
    }
}


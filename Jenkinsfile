pipeline {
    agent {
        node {
        label 'AGENT-1'
        }
    }
    environment {
        appVersion = ''
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
        stage('Test') {
            steps {
                script {
                    sh """
                        echo "Hello Testing..."
                        env
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


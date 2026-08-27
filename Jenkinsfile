pipeline {
    agent any
    stages {
        stage('Variables Demo') {
            steps {
                script {
                    def appName = 'MyApplication'
                    def port = '8080'
                    def isProduction = false
                    echo appName
                    echo port
                    echo isProduction.toString()   // fix here
                }
            }
        }
        stage('String Operations') {
            steps {
                script {
                    def message = 'Jenkins Pipeline Tutorial'
                    echo "Length: ${message.length()}"
                }
            }
        }
    }
}
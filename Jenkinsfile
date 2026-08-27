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
        stage('Build Version') {
            steps {
                script {
                    def major = '1'
					def minor = '0'
					def version = "${major}.${minor}.${env.BUILD_NUMBER}"
					env.APP_VERSION = version 
					echo "Application version: ${version}"
                }
            }
        }
        stage('Display Version') {
            steps {
                script {
					echo "Using version: ${env.APP_VERSION}"
					def imageName = "myapp:${env.APP_VERSION}"
					echo "Docker image would be: ${imageName}"
                }
            }
        }
        stage('Jenkins Info') {
            steps {
                script {
					echo "Buld number: ${env.BUILD_NUMBER}"
					echo "Buld id: ${env.BUILD_ID}"
					echo "Job name: ${env.JOB_NAME}"
					echo "Workspace: ${env.WORKSPACE}"
					echo "Build url: ${env.BUILD_URL}"
                }
            }
        }		
    }
}
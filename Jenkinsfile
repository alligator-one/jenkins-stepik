// Jenkinsfile
// Мой первый пайплайн

pipeline {
    agent any
    stages {
        stage('Prepare') {
            steps {
                echo 'Stage Prepare'
                sh 'mkdir -p build logs temp'
                echo 'Directories created'				
            }
        }

        stage('Build') {
            steps {
                echo 'Stage Build'
                sh 'echo "Build version: 1.0.0" > build/version.txt'
				sh 'date >> build/version.txt'
				echo 'Build completed'
            }
        }
		stage('Verify') {
			steps {
				echo "Verifying build..."
                sh 'cat build/version.txt'
				sh 'ls -la build/'
				echo "Verification completed"
			}
		}
		stage('System Info') {
			steps {
				echo "System Info"
                sh 'whoami'
				sh 'df -h .'
				echo "Build Number: ${BUILD_NUMBER}"
				echo "Build Number: ${JOB_NAME}"				
			}
		}		
    }
}

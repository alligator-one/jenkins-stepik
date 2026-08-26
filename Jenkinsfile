pipeline {
    agent none
    stages {
        stage('Check Agent') {
            agent any
			steps {
                echo 'Running on agent'
                sh 'hostname'
                echo "Build Number: ${env.WORKSPACE}"
				echo "Build Number: ${env.NODE_NAME}"
            }
        }
        stage('Build Info') {
            agent any
			steps {
                echo 'Build information...'
                echo "Build Number: ${env.BUILD_NUMBER}"
				echo "Build Number: ${env.BUILD_ID}"
                echo "Build Number: ${env.BUILD_URL}"				
            }
        }

    }
}

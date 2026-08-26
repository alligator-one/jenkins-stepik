pipeline {
    agent any
    stages {
        stage('Check Agent') {
            steps {
                echo 'Running on agent'
                sh 'hostname'
                echo "Build Number: ${env.WORKSPACE}"
				echo "Build Number: ${env.NODE_NAME}"
            }
        }


    }
}

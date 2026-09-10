pipeline {
    agent any

    stages {
        stage ('Checkout') {
            steps {
                echo "Checkout"
            }
        }
        stage ('Build') {
            steps {
                echo "Building...!"
            }
        }
        stage ('Test') {
            steps {
                echo "Testing"
            }
        }
        stage ('Deploy') {
            when {
                branch 'main'
            }
            steps {
                echo "Deploying"
            }
        }
    }

    post {
        always {
            echo 'This always runs'
            archiveArtifacts allowEmptyArchive: true, artifacts: '**/reports/**, **/test-results/**'
        }
        success {
            echo 'The build passed'
        }
        failure {
            echo 'The build failed'
        }
    }
}

pipeline {
    agent any
    stages {
        stage('Build') {
            steps { echo 'Building' }
        }
        stage('Tests') {
            parallel {
                stage('Unit') { steps { sh 'echo Unit tests' } }
                stage('Integration') { steps { sh 'echo Integration tests' } }
            }
        }
    }
}

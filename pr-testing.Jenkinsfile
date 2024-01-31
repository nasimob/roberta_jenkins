pipeline {
    agent any
    stages {
        stage('Unittest') {
            steps {
                echo "testing"

                sh '''
                pip install pytest
                python3 -m pytest --junitxml results.xml tests
                '''
            }
            post {
                always {
                junit allowEmptyResults: true, testResults: 'results.xml'
                }
            }

        }
        stage('Lint') {
            steps {
                echo "linting"
            }
        }
        stage('Functional test') {
            steps {
                echo "testing"
            }
        }
    }
}

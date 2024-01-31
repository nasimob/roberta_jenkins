pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'ls'
                sh 'echo building...'
                sh '''
                    echo "Multiline shell steps works too"
                    ls -lah
                    docker build -t roberta_image .
                '''
            }
        }
    }
}

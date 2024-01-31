pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'ls'
                sh 'echo building...'
                sh '''
                    aws ecr get-login-password --region eu-north-1 | docker login --username AWS --password-stdin 933060838752.dkr.ecr.eu-north-1.amazonaws.com
                    docker build -t nasim_roberta .
                    docker tag nasim_roberta:latest 933060838752.dkr.ecr.eu-north-1.amazonaws.com/nasim_roberta:${env.BUILD_ID}
                    docker push 933060838752.dkr.ecr.eu-north-1.amazonaws.com/nasim_roberta:${env.BUILD_ID}
                '''
            }
        }
    }
}

pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'ls'
                sh 'echo building...'
                sh '''
                    aws ecr get-login-password --region eu-north-1 | docker login --username AWS --password-stdin 933060838752.dkr.ecr.eu-north-1.amazonaws.com
                    docker build -t nasim_roberta:${BUILD_NUMBER} .
                    docker tag nasim_roberta:${BUILD_NUMBER} 933060838752.dkr.ecr.eu-north-1.amazonaws.com/nasim_roberta:${BUILD_NUMBER}
                    docker push 933060838752.dkr.ecr.eu-north-1.amazonaws.com/nasim_roberta:${BUILD_NUMBER}
                '''
            }
        }
        stage('Trigger Deploy') {
            steps {
                build job: 'RobertaDeploy', wait: false, parameters: [
                    string(name: 'ROBERTA_IMAGE_URL', value: "933060838752.dkr.ecr.eu-north-1.amazonaws.com/nasim_roberta:25")
                ]
              }
        }

    }
}

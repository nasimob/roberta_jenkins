pipeline {
    environment {
    ECR_URL='933060838752.dkr.ecr.eu-north-1.amazonaws.com'
    IMAGE_NAME = 'nasim_roberta'
    }
    agent {

        docker {

            image '${ECR_URL}/nasim_jenkins.agent:latest'
            args  '--user root -v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'ls'
                sh 'echo building...'
                sh '''
                    aws ecr get-login-password --region eu-north-1 | docker login --username AWS --password-stdin 933060838752.dkr.ecr.eu-north-1.amazonaws.com
                    docker build -t $IMAGE_NAME:${BUILD_NUMBER} .
                    docker tag $IMAGE_NAME:${BUILD_NUMBER} $ECR_URL/$IMAGE_NAME:${BUILD_NUMBER}
                    docker push $ECR_URL/$IMAGE_NAME:${BUILD_NUMBER}
                '''
            }
        }
        stage('Clean Workspace After Build') {
            steps {
                cleanWs() // Clean the workspace after this stage
            }
        }
        stage('Trigger Deploy') {
            steps {
                build job: 'RobertaDeploy', wait: false, parameters: [
                    string(name: 'ROBERTA_IMAGE_URL', value: "${ECR_URL}/${IMAGE_NAME}:${BUILD_NUMBER}")
                ]
              }
        }
    }
}

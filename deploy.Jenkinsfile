pipeline {
    agent {
            docker {
            label 'general'
            image '933060838752.dkr.ecr.eu-north-1.amazonaws.com/nasim_jenkins.agent:latest'
            args  '--user root -v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    parameters { string(name: 'ROBERTA_IMAGE_URL', defaultValue: '', description: '') }
    stages {
        stage('Deploy') {
            steps {
                // complete this code to deploy to real k8s cluster
                sh '# kubectl apply -f ....'
            }
            post {
            always {
                script {
                    // Cleanup Docker images
                    sh 'docker image prune -af'
                }
            }
        }
        }
    }
}

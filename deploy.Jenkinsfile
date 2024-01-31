pipeline {
    agent any
    parameters { string(name: 'ROBERTA_IMAGE_URL', defaultValue: '', description: '') }
    string(name: 'ROBERTA_IMAGE_URL', value: "933060838752.dkr.ecr.eu-north-1.amazonaws.com/nasim_roberta:25")
    stages {
        stage('Deploy') {
            steps {
                // complete this code to deploy to real k8s cluster
                sh '# kubectl apply -f ....'
            }
        }
    }
}

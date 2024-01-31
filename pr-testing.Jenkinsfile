pipeline {
    agent any
    environment {
    ECR_URL='933060838752.dkr.ecr.eu-north-1.amazonaws.com'
    IMAGE_NAME = 'nasim_roberta'
    }
    stages {
        stage('Unittest') {
            steps {
                echo "testing"

                sh '''
                pip install pytest
                pip install pylint
                pip install flask transformers textstat
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
          sh 'python3 -m pylint -f parseable --reports=no *.py > pylint.log'
        }

        post {
          always {
            sh 'cat pylint.log'
            recordIssues (
              enabledForFailure: true,
              aggregatingResults: true,
              tools: [pyLint(name: 'pylint', pattern: '**/pylint.log')]
            )
          }
        }

        }
        stage('Functional test') {
            steps {
                echo "testing"
                sh '''
                docker run -d -p 8081:8081 -p 50000:50000 --name Roberta_container $ECR_URL/$IMAGE_NAME:37
                // Wait for the container to start (adjust sleep time as needed)
                sleep 10
                        // Perform functional tests
                curl "localhost:8081/analyze?text=Intel%20is%20happy%20and%20excited%20to%20launch%20the%20new%20generation%20of%20processors"
                curl "localhost:8081/analyze?text=This%20is%20terrible%20movie."
                curl "localhost:8081/analyze?text=This%20is%20a%20neutral%20statement"

                def expected1 = 'positive labels: optimism, excitement'
                def expected2 = 'negative labels: disappointment, disgust, fear'
                def expected3 = 'neutral labels: neutral'

                def result1 = readFile('result1.txt').trim()
                def result2 = readFile('result2.txt').trim()
                def result3 = readFile('result3.txt').trim()

                // Compare results to expected sentiments
                assert result1.contains(expected1)
                assert result2.contains(expected2)
                assert result3.contains(expected3)

                echo "Result 1: $result1"
                echo "Result 2: $result2"
                echo "Result 3: $result3"

                // Cleanup: Stop and remove the container
                docker stop  Roberta_container
                docker rm  Roberta_container
                docker
                '''


            }
        }
    }
}

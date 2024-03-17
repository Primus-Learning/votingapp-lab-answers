// continuous delivery pipeline
def region = "us-east-1"

pipeline{
    agent any
    stages{
        stage("Checkout deployment"){
            steps{
                script{
                    git branch: 'main', credentialsId: 'github', url: 'https://github.com/Primus-Learning/votingapp-lab-answers.git'
                }
            }
        }

        stage("Deploy to Dev"){
            steps{
                script{
                    withAWS(region:"$region",credentials:'aws_creds'){
                        sh "aws eks update-kubeconfig --name vote-dev"
                        sh 'curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.28.5/2024-01-04/bin/linux/amd64/kubectl'  
                        sh 'chmod u+x ./kubectl'
                        sh "ls -l"
                        sh "./kubectl apply -f k8s-specifications -n vote "
                        sh "./kubectl apply -f ingress -n vote "
                    }
                }
            }
        }
    }
}


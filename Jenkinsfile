// continuous delivery pipeline
def region = "us-east-1"
def registry= "940090592876.dkr.ecr.us-east-1.amazonaws.com"


pipeline{
    agent any
    parameters { 
        string(name: 'resultTag', defaultValue: 'latest', description: 'docker tag for result ms') 
        string(name: 'voteTag', defaultValue: 'latest', description: 'docker tag for vote ms') 
        string(name: 'workerTag', defaultValue: 'latest', description: 'docker tag for worker ms') 
        }
    stages{
        stage("Deploy to Dev"){
            when{branch 'develop'}
            steps{
                script{
                    withAWS(region:"$region",credentials:'aws_creds'){
                        sh "aws eks update-kubeconfig --name vote-dev"
                        sh 'curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.28.5/2024-01-04/bin/linux/amd64/kubectl'  
                        sh 'chmod u+x ./kubectl'
                        sh "kubectl set image deploy/result result=${registry}/result:${params.resultTag} -n vote "
                        sh "kubectl set image deploy/vote vote=${registry}/vote:${params.voteTag} -n vote "
                        sh "kubectl set image deploy/worker worker=${registry}/worker:${params.workerTag} -n vote "
                        sh "kubectl rollout restart deploy/result deploy/worker deploy/vote -n vote"
                    }
                }
            }
        }
    }
}


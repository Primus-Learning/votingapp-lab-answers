// continuous delivery pipeline
def region = "us-east-1"

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
                        sh "kubectl set image deploy/result result=${params.resultTag} -n vote "
                        sh "kubectl set image deploy/vote result=${params.voteTag} -n vote "
                        sh "kubectl set image deploy/worker result=${params.workerTag} -n vote "
                        sh "kubectl rollout restart deploy/result deploy/worker deploy/vote -n vote"
                    }
                }
            }
        }
    }
}


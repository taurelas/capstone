pipeline {
  agent any
  stages {
    stage('Updating config file') {
      steps {
        withAWS(region: 'us-west-2', credentials: 'eks-user') {
          sh '''aws eks --region us-west-2 update-kubeconfig --name capstone
docker image history mbeimcik/capstone'''
        }
      }
    }

    stage('Rolling update K8S') {
      steps {
        withAWS(region: 'us-west-2', credentials: 'eks-user') {
         sh '''
           TAG=$(git log Development -1 --pretty=%h)
           kubectl set image deployment capstone capstone=mbeimcik/capstone:${IMG}'''
        }
      }
    }

    stage('Rolling update status') {
      steps {
        withAWS(region: 'us-west-2', credentials: 'eks-user') {
          sh 'kubectl rollout status deployment capstone'
        }
      }
    }
  }
}

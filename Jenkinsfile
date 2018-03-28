pipeline {
  agent any
  
  stages {
    stage('Build') {
      environment {
        // 'This value is exported to all commands in this stage'
        IMAGE_TAG = "${env.BRANCH_NAME}"
        HOST_BUILD = ":2375"
        IMAGE_NAME = ":5000//:$IMAGE_TAG"
      }
      
      steps {
        echo 'Building...'

        sh 'echo "HI"'

        sh 'docker -H $HOST_BUILD build -t $IMAGE_NAME .'
        sh 'docker -H $HOST_BUILD push $IMAGE_NAME'
        sh 'docker -H $HOST_BUILD rmi $IMAGE_NAME'
      }
    }
  }
}

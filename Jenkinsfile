pipeline {
  agent any
  
  stages {
    stage('Build') {
      environment {
        // 'This value is exported to all commands in this stage'
        IMAGE_TAG = "${env.BRANCH_NAME}"
        HOST_BUILD = ":2375"
        IMAGE_NAME = ":5000//:${env.BRANCH_NAME}"
      }
      
      steps {
        echo 'Building...'

        sh 'echo "HI"'
        sh "echo ${env.BUILD_TAG}"
        sh "echo ${env.BRANCH_NAME}"

        sh "docker build -t jimlambie/${env.BUILD_ID} ."
        sh "docker push jimlambie/${env.BUILD_ID}"
        sh "docker rmi jimlambie/${env.BUILD_ID}"
      }
    }
  }
}

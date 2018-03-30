pipeline {
  agent any

  stages {
    stage('Build') {
      environment {
        IMAGE_TAG = "${env.BRANCH_NAME}"
        HOST_BUILD = ":2375"
        IMAGE_NAME = ":5000//:${env.BRANCH_NAME}"

        GIT_COMMIT_EMAIL = sh (
          script: 'git --no-pager show -s --format=\'%ae\'',
          returnStdout: true
        ).trim()

        BRANCH_TAG = sh (
          script: "echo ${env.BRANCH_NAME} | tr '/' '-'",
          returnStdout: true
        ).trim()
      }
      
      steps {
        echo 'Building...'

        sh 'echo "HI"'
        sh "echo ${env.BUILD_TAG}"
        sh "echo ${env.BRANCH_NAME}"

        sh "echo $GIT_COMMIT_EMAIL"
        sh "echo $BRANCH_TAG"

        sh "docker build -t ${env.BUILD_TAG} ."
        sh "docker tag friendlyhello jimlambie/${env.BUILD_TAG}"
        sh "docker push jimlambie/${env.BUILD_TAG}"
        sh "docker rmi jimlambie/${env.BUILD_TAG}"
      }
    }

    stage('Deploy') {
      steps {
        echo 'Deploying...'

        sh "docker pull jimlambie/${env.BUILD_TAG}"
        sh "docker run -d --restart=always --name 'hello' -e NODE_ENV=test -p 50000:3001 jimlambie/${env.BUILD_TAG}"
      }
    }
  }
}

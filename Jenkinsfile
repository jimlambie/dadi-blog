pipeline {
  agent any

  stages {
    stage('Build') {
      environment {
        IMAGE_TAG = "${env.JOB_NAME.toLowerCase()}"
        HOST_BUILD = ":2375"
        IMAGE_NAME = ":5000//:${env.BRANCH_NAME}"

        GIT_COMMIT_EMAIL = sh (
          script: 'git --no-pager show -s --format=\'%ae\'',
          returnStdout: true
        ).trim()

        BRANCH_TAG = sh (
          script: "echo '${env.BRANCH_NAME}' | tr '/' '-'",
          returnStdout: true
        ).trim()
      }
      
      steps {
        script {
          env.BID = sh (
            script: "echo '${env.BRANCH_NAME}' | tr '/' '-'",
            returnStdout: true
          ).trim()
        }

        echo 'Building...'

        sh 'echo "HI"'
        sh "echo ${env.BUILD_TAG.toLowerCase()}"
        sh "echo ${env.BRANCH_NAME}"

        sh "echo $GIT_COMMIT_EMAIL"
        sh "echo ${env.BID.toLowerCase()}"
        sh "echo ${env.JOB_NAME.toLowerCase()}"
        

        sh "docker build -t ${env.BUILD_TAG.toLowerCase()} ."
        sh "docker tag ${env.BUILD_TAG.toLowerCase()} jimlambie/${env.BUILD_TAG.toLowerCase()}"
        sh "docker push jimlambie/${env.BUILD_TAG.toLowerCase()}"
        sh "docker rmi jimlambie/${env.BUILD_TAG.toLowerCase()}"
      }
    }

    stage('Deploy') {
      steps {
        echo 'Deploying...'

        sh "docker pull jimlambie/${env.BUILD_TAG.toLowerCase()}"
        sh "docker run -d --restart=always --name 'hello' -e NODE_ENV=test -e VIRTUAL_HOST=${env.BUILD_TAG.toLowerCase()}.mustdash.es -p 3001:3001 jimlambie/${env.BUILD_TAG.toLowerCase()}"

        slackSend color: "good", message: "${env.JOB_NAME} deployed. Test it here: http://${env.BUILD_TAG.toLowerCase()}.mustdash.es"
      }
    }
  }
}

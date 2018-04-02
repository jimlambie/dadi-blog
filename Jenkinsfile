pipeline {
  agent any

  stages {
    stage('Build') {
      environment {
        IMAGE_TAG = sh (
          script: "echo '${env.JOB_NAME.toLowerCase()}' | tr '/' '-'",
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

        sh "echo ${env.BID.toLowerCase()}"
        sh "echo ${IMAGE_TAG.toLowerCase()}"
        

        sh "docker build -t ${IMAGE_TAG} ."
        sh "docker tag ${IMAGE_TAG} jimlambie/${IMAGE_TAG}"
        sh "docker push jimlambie/${IMAGE_TAG}"
        sh "docker rmi jimlambie/${IMAGE_TAG}"
      }
    }

    stage('Deploy') {
      steps {
        echo 'Deploying...'

        sh "docker pull jimlambie/${IMAGE_TAG}"
        sh "docker run -d --restart=always --name 'hello' -e NODE_ENV=test -e VIRTUAL_HOST=${IMAGE_TAG}.mustdash.es -p 3001:3001 jimlambie/${IMAGE_TAG}"

        slackSend color: "good", message: "${env.JOB_NAME} deployed. Test it here: http://${IMAGE_TAG}.mustdash.es"

        input message: 'Finished testing? (Click "Proceed" to continue)'
        sh "docker ps -f name=hello -q | xargs --no-run-if-empty docker container stop"
        sh "docker container ls -a -fname=hello -q | xargs -r docker container rm"
      }
    }
  }
}

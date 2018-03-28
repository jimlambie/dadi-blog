pipeline {
  agent {
    docker {
      image 'jwilder/nginx-proxy'
      label BRANCH_NAME
      args '-e VIRTUAL_HOST=${BRANCH_NAME}.local -p 80:80 -v /var/run/docker.sock:/tmp/docker.sock:ro'
    }
  }
  stages {
    stage('Build') {
      steps {
        echo 'Building...'
      }
    }
  }
}

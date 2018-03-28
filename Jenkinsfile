pipeline {
  agent {
    docker {
      image 'jwilder/nginx-proxy'
      label BRANCH_NAME
    }
    
  }
  stages {
    stage('Build') {
      steps {
        echo 'Building...'
        input 'Hi'
      }
    }
  }
}
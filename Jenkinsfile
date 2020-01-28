pipeline {
  agent {
    node { label 'docker && linux && !gpu' }
  }
  triggers{
    upstream(
      upstreamProjects: 'prova',
      threshold: hudson.model.Result.SUCCESS)
  }
  stages {

    stage('Configure') {
      steps {
        sh 'git fetch --tags'
        sh 'printenv'
      }
    }
  }

  post {
    always {
      echo 'One way or another, I have finished'
    }
    success {
      echo "OK"
    }
    unstable {
      echo 'I am unstable :/'
    }
    failure {
      echo 'I failed :('
    }
    cleanup {
      echo "Cleaning"
    }
  } 
}
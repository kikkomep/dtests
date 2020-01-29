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
        script {
          currentBuild.upstreamBuilds?.each { b ->
              echo b.getFullProjectName()
              for (Map e : b.getBuildVariables()) {
                println e
              }
              echo "Printing Environment..."
              def rb = b.getRawBuild()
              for(Map e: rb.getEnvironment()){
                println e
              }
          }
        }
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
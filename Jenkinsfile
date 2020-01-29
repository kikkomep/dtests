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
            b.getBuildVariables().each {
              key, value -> L:{
                println "$key: $value"
                if ( key == "GIT_BRANCH"){
                  myVar = "$value"
                }
              }
            }
            for (Map e : b.getBuildVariables()) {
              println "$e.key ==> $e.value"
              if ( e.$key == "GIT_BRANCH"){
                
              }
            }
          }
        }
      }
    }


    stage('Use configured variable') {
      steps {
        sh 'echo $myVar'
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
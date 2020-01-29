myVar = "Initial value"

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
        echo "${myVar}"
        script {
          currentBuild.upstreamBuilds?.each { b ->
            echo b.getFullProjectName()
            b.getBuildVariables().each {
              key, value -> L:{
                println "$key: $value"
                if ( "$key" == "GIT_BRANCH"){
                  myVar = "$value"
                  echo "$value --- $myVar"
                }
              }
            }
            // for (Map e : b.getBuildVariables()) {
            //   println "$e.key ==> $e.value"
            // }
          }
        }
      }
    }


    stage('Use configured variable') {
      when {
        expression { return "$myVar" == "origin/master" }
      }

      steps {
        sh "echo ${myVar}"
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
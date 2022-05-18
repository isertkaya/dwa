pipeline {
  agent any
  stages {
    stage('Setting Proper Permission') {
      steps {
        sh 'sudo chown root:jenkins /run/docker.sock'
      }
    }

    stage('Maven Build') {
      agent {
        docker {
          image 'maven'
          reuseNode true
        }

      }
      steps {
        sh 'mvn clean  package'
        archiveArtifacts(allowEmptyArchive: true, artifacts: '**/targets/*.war')
      }
    }

  }
}
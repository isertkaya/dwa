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
        archiveArtifacts(allowEmptyArchive: true, artifacts: '**/target/*.war')
      }
    }

    stage('SonarScanner') {
      agent {
        docker {
          image 'sonarsource/sonar-scanner-cli'
        }

      }
      environment {
        SONAR_LOGIN = '45bb57a590fe12b5b78c7a41dd7d95c4d95e90dd'
        SONAR_HOST_URL = 'https://sonar.isertkaya.de'
      }
      steps {
        ws(dir: 'dwa_master')
        sh 'sonar-scanner -Dsonar.sources=. -Dsonar.language=java -Dsonar.java.binaries=**/target/classes -Dsonar.host.url=$SONAR_HOST_URL -Dsonar.projectKey=myproject -Dsonar.login=$SONAR_LOGIN '
      }
    }

  }
}
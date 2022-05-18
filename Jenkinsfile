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
        sh 'sonar-scanner -Dsonar.projectBaseDir=../dwa_master -Dsonar.sources=../dwa_master/. -Dsonar.language=java -Dsonar.java.binaries=../dwa_master/target/ -Dsonar.host.url=$SONAR_HOST_URL -Dsonar.projectKey=myproject -Dsonar.login=$SONAR_LOGIN -Dsonar.qualitygate.wait=true '
      }
    }

  }
}
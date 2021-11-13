pipeline {

  agent {
    docker {
      image 'maven:3.8.1-adoptopenjdk-11'
      args '-v $HOME/.m2:/root/.m2'
    }
  }

  stages {
    stage('Build') {
      steps {
        echo 'Building..'
      }
    }
    stage('Test') {
      steps {
        echo 'Testing..'
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
        }
      }
    }
    stage('Push') {
      steps {
        echo 'Pushing....'
      }
    }
  }

}

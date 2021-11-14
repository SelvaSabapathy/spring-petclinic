pipeline {

  agent any

  stages {
    stage('Build') {
      agent {
        docker {
          image 'maven:3.8.3-openjdk-11'
          args '-v $HOME/.m2:/root/.m2'
        }
      }
      steps {
        echo 'Compile and package the application...'
        sh 'mvn -DskipTests clean package'
      }
    }
    stage('Test') {
      agent {
        docker {
          image 'maven:3.8.3-openjdk-11'
        }
      }
      steps {
        echo 'Test the application...'
        sh 'mvn test'
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
        }
      }
    }
    stage('Package') {
      steps {
        echo 'Dockerize the application...'
      }
    }
    stage('Push') {
      steps {
        echo 'Pushing dockerized application to Artifactory...'
      }
    }
  }

}

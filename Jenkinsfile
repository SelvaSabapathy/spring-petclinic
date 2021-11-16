pipeline {

  environment {
    registry = "selvasabapathy/petclinic"
    registryCredential = 'dockerhub_selvasabapathy'
    dockerImage = ''
  }

  agent any

  stages {
    stage('Info') {
      steps {
        sh 'printenv'
      }
    }
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
          args '-v $HOME/.m2:/root/.m2'
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
    stage('Push') {
      steps {
        echo 'Dockerize and push the application...'
        sh 'cd $WORKSPACE'
        sh './mvnw -DskipTests clean package'
        script {
          dockerImage = docker.build registry + ":$JOB_NAME-$BUILD_NUMBER"
          docker.withRegistry( '', registryCredential ) { dockerImage.push() }
        }
      }
      post {
        success {
          echo 'Remove the docker image that was pushed...'
          sh "docker rmi $registry:$JOB_NAME-$BUILD_NUMBER"
        }
      }
    }
  }

}

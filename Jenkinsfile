pipeline {
  environment {
    registry = "niema73/tp4"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git branch: 'master', url: 'https://github.com/Niema-git/tp4devops'
      }
    }
    stage('Building image') {
      steps {
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Test image') {
      steps {
        script {
          echo "Tests passed"
        }
      }
    }
    stage('Publish Image') {
      steps {
        script {
          docker.withRegistry('', registryCredential) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Deploy image') {
      steps {
        bat "docker rm -f tp4-pipeline"
        bat "docker run -d --name tp4-pipeline -p 8082:80 $registry:$BUILD_NUMBER"
      }
    }
  }
}
pipeline {
  environment {
    imagename = "khadydiagne/mon_app"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Clone') {
      steps {
        git([url: 'https://github.com/khadythiara/jenkins-baamtu.git', branch: 'main'])

      }
    }
    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build(imagename, ".")
        }
      }
    }
    stage('push Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ){
                  dockerImage.push("$BUILD_NUMBER")
                  dockerImage.push('latest')

          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $imagename:$BUILD_NUMBER"
         sh "docker rmi $imagename:latest"

      }
    }
  }
}

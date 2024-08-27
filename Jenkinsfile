pipeline {
  environment {
    imagename = "khadydiagne/push_jenkins"
    registryCredential = 'simple-java-project'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git([url: 'https://github.com/khadythiara/jenkins-baamtu.git', branch: 'main'])

      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build(imagename, ".")
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push("$BUILD_NUMBER")
             dockerImage.push('latest')

          }
        }
      }
    }
    
    stage('Remove Existing Container') {
    steps {
        script {
            def containerExists = sh(script: "docker ps -a --filter 'name=simple_app_java' --format '{{.Names}}'", returnStdout:          
            true).trim()
            if (containerExists) {
                sh 'docker rm -f simple_app_java'
            }
        }
    }
}
    
     stage('Run Docker Container') {
          steps {
              script {
                    // Exécution du conteneur Docker
                    dockerImage.run("-d -p 8080:80")
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

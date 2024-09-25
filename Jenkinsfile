pipeline {
  environment {
    imagename = "khadydiagne/simple-java-app"
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
       stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh './mvnw sonar:sonar -Dsonar.projectKey=test_java -Dsonar.java.binaries=target/sonar'
                }
            }
        }
    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push("$BUILD_NUMBER")
             dockerImage.push('latest')

          }
        }
      }
    }
    
 
     stage('Run Docker Container') {
          steps {
              script {
                    // Exécution du conteneur Docker
                    dockerImage.run("-d -p 8087:80")
                }
            }
        }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi -f $imagename:$BUILD_NUMBER"
         sh "docker rmi -f $imagename:latest"

      }
    }
  }
}

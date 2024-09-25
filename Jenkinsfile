pipeline {
  environment {
    imagename = "khadydiagne/simple-java-app"
    registryCredential = 'simple-java-project'
    dockerImage = ''

    // Propriétés SonarQube
    SONAR_PROJECT_KEY = 'TEST'
    SONAR_HOST_URL = 'http://localhost:9000/'
    SONAR_TOKEN = credentials('sonarqube') // Jeton SonarQube
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git([url: 'https://github.com/khadythiara/jenkins-baamtu.git', branch: 'main'])
      }
    }

    stage('Building image') {
      steps {
        script {
          dockerImage = docker.build(imagename, ".")
        }
      }
    }

    stage('SonarQube Analysis') {
      steps {
        script {
          def mvn = tool 'Default Maven';
          withSonarQubeEnv() {
            sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=${SONAR_PROJECT_KEY} -Dsonar.projectName='TEST'"
          }
        }
      }
    }

    stage('Push Image') {
      steps {
        script {
          docker.withRegistry('', registryCredential) {
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
      steps {
        sh "docker rmi -f $imagename:$BUILD_NUMBER"
        sh "docker rmi -f $imagename:latest"
      }
    }
  }
}

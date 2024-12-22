pipeline {
    environment {
        imagename = "khadydiagne/simple-java-app"
        registryCredential = 'dockerhub'
        dockerImage = ''
        BUILD_NUMBER = "${env.BUILD_NUMBER}"
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

    }
}

pipeline {
    agent any
    stages {
    stage('clone') {
         steps {
            // Remove the previous directory if it exists
            sh 'rm -rf simple-java-project'
            
            // Clone the repository using Git
            git branch: 'main', url: 'https://github.com/khadythiara/jenkins-baamtu.git'

    }
}
stage('build') {
    steps {
        //sh 'ls -la'
        //sh 'cd simple-java-project/src/main/java/works/buddy/samples && javac WorksWithHerokuServlet.java'
        //sh 'mvn -version'
        //sh './mvnw clean compile'
        sh 'mvn clean compile'
        //sh 'mvn clean package'
        //sh 'mvn clean'
        //sh 'mvn dependency:tree'

    }
}

    stage('run'){
            steps {
                sh 'ls -la'
                sh 'pwd'
                //sh "cd target/classes/works/buddy/samples/ && java works.buddy.samples.WorksWithHerokuServlet"
                sh 'java -cp target/classes WorksWithHerokuServlet'
'
                
            }
            }

}
}

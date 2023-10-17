pipeline {
    agent any
     stages {
        stage('Clone sources') {
            steps {
                git branch: 'Dev', url: 'https://github.com/nakkinasai/New.git'
            }
        }
         
        stages {
        stage('Clean Compile') {
            steps {
                
                // Clean and compile.
                sh "mvn clean compile" }
        }
        
        stage('Install') {
            steps {
                
                
                sh "mvn install"

            }
        }
        stage("build & SonarQube analysis") {
             steps {
              withSonarQubeEnv('sonarserver') {
                sh '''mvn sonar:sonar \
                -Dsonar.projectKey=four \
                -Dsonar.host.url=http://13.233.204.68:9000 /
                -Dsonar.login=5c2c436d1dc5d49d7cee67a87e472aa3f03b967b'''
              }
            }
          }
}
}
}

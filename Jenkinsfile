pipeline {
    agent any
     stages {
        stage('Clone sources') {
            steps {
                git branch: 'dev', url: 'https://github.com/nakkinasai/New.git'
            }
        }
         stage('Clean Compile') {
            steps {
                
                // Clean and compile.
                sh "mvn clean compile"

            }
        }
         
        stage('Test') {
            steps {
                // Test Cases.
                sh "mvn test"
            }
        }
        stage('Install') {
            steps {
         
             }
         }
         stage("build & SonarQube analysis") {
             steps {
              withSonarQubeEnv('sonarserver') {
                sh '''mvn sonar:sonar \
                -Dsonar.projectKey=two \
                -Dsonar.host.url=http://13.232.138.93:9000 /
                -Dsonar.login=5c2c436d1dc5d49d7cee67a87e472aa3f03b967b'''
              }
            }
          }
}
}

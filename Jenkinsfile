pipeline {
    agent any
    tools{
        maven "maven3"
    }
     stages {
        stage('Clone sources') {
            steps {
                git branch: 'Dev', url: 'https://github.com/nakkinasai/New.git'
            }
        }
         stage ('maven build'){
             steps{ sh" mvn clean install" }
         }
         stage("build & SonarQube analysis") {
             steps {
              withSonarQubeEnv('sonarserver') {
                sh '''mvn sonar:sonar \
                -Dsonar.projectKey=five \
                -Dsonar.host.url=http://13.232.138.93:9000 /
                -Dsonar.login=5c2c436d1dc5d49d7cee67a87e472aa3f03b967b'''
              }
            }
          }
}
}





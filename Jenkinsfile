pipeline {
    agent any
     stages {
        stage('Clone sources') {
            steps {
                git branch: 'Dev', url: 'https://github.com/nakkinasai/New.git'
            }
        }
         
        stage("Build Application"){
            steps {
                sh "mvn clean package"
            }

       }

       stage("Test Application"){
           steps {
                 sh "mvn test"
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

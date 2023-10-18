pipeline
{
    agent any 
    parameters {
        choice(choices: 'dev\nProd', description: 'Select Branch to build', name: 'branch')
    }
//    environment {
        //PATH ="/opt/maven/apache-maven-3.9.5:$PATH"
  //   }
    tools{
        maven "maven-3.9.5"
    }
    stages
    {
        stage("Git Checkout"){
            steps{
                git credentialsId: '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', url: 'https://github.com/nakkinasai/New.git' , branch: "${params.branch}"
            }
        }
    stage("Maven Build"){
            steps{
               sh" mvn -f New/pom.xml clean install"
            }
        }
    stage("Unit Testing"){
            steps{
                sh "mvn -f New/pom.xml test" 
            }
        }
        stage ("Testing Results"){
            steps{
                junit'*/**/*.xml'
            }
        }
    stage ("Sonar Analysis "){
            steps{
                
                withSonarQubeEnv('sonarqube-latest')
                {
                    sh "mvn -f New/pom.xml sonar:sonar  -Dsonar.projectKey='four' -Dsonar.projectName='four'"
                }
            }
            
        }
     stage("Artifactory_Upload"){
           steps{
               rtUpload (
                   serverId: 'artifactory',
                            spec: '''{
                            "files": [
                            {
                            "pattern": "*.war",
                            "target": "miniasg/"
                                
                            }
                        ]
                                
                    }''',
                )
                           
                rtPublishBuildInfo(
                        serverId:"artifactory"
                        )
                }
        }
    }
}




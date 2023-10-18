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
                git credentialsId: '8e2af35e-b19f-4ca3-91a7-430a20824461', url: 'https://github.com/nakkinasai/New.git' , branch: "${params.branch}"
            }
        }
    stage("Maven Build"){
            steps{
               sh" mvn -f Asg/pom.xml clean install"
            }
        }
    stage("Unit Testing"){
            steps{
                sh "mvn -f Asg/pom.xml test" 
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
                    sh "mvn -f Asg/pom.xml sonar:sonar  -Dsonar.projectKey='Dev_Analysis' -Dsonar.projectName='Dev_Analysis'"
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




pipeline
{
    agent any 
    parameters {
        choice(choices: 'dev\nPro', description: 'Select Branch to build', name: 'branch')
    }

    tools{
        maven "maven3"
    }
    stages
    {
        stage("Git Checkout"){
            steps{
                git credentialsId: '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', url: 'https://github.com/nakkinasai/New.git' , branch: "${params.branch}"
            }
        }
     stage ('maven build'){
             steps{ sh" mvn clean install" }
            }
        }
    
    stage ("Sonar Analysis "){
            steps{
                
                withSonarQubeEnv('sonarqube-latest')
                {
                    sh "mvn sonar:sonar  -Dsonar.projectKey='create' -Dsonar.host.url='http://13.127.158.147:9000'  -Dsonar.login='1bb1652d18f5b800fb0148bd7a13c7593f7b9aab'"
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
    
post{
        always{
            mail to: "sid.demo08@gmail.com",
            subject: "Status - Jenkins Job with Build #${BUILD_NUMBER}",
            body: "Jenkins Job with Build #${BUILD_NUMBER}. Please go to ${BUILD_URL} and verify the build"
        }
    }
}

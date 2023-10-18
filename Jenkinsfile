pipeline {
    agent any
    tools{
        maven "maven3"
    }
     stage("Git Checkout"){
            steps{
                git credentialsId: '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', url: 'https://github.com/nakkinasai/New.git' , branch: "${params.branch}"
            }
        }
         stage ('maven build'){
             steps{ sh" mvn clean install" }
         }
         stage("SonarQube Analysis"){
           steps {
	           script {
		        withSonarQubeEnv(credentialsId: 'jenkins-sonarqube') { 
                        sh "mvn sonar:sonar"
              }
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
	      stage ('Deployments') {
		      parallel{
			      stage ("Deploy to Staging") {
				      steps {
					      sh "scp -v -o StrictHostKeyChecking=no **/*.war root@${params.tomcat_staging}:/tomcat/webapps/"
			  
				  
 }
}

}
 }

}



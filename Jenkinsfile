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
					      sh "scp -v -o StrictHostKeyChecking=no **/*.war root@${params.tomcat_staging}:/tomcat/webappa/"
			  
				  
 }
}

}
 }

}
}

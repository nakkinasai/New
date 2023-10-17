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
	      stage("deploy-dev"){
       steps{
          sshagent(['tomcat-dev1']) {
          sh """
          scp -o StrictHostKeyChecking=no target/MyWebApp.war  
          Linux@13.126.76.37:/opt/tomcat/webapps/
          ssh Linux@13.126.76.37 /opt/tomcat/bin/shutdown.sh
          ssh Linux@13.126.76.37 /opt/tomcat/bin/startup.sh
           """
			  
				  
 }
}

}
 }

}

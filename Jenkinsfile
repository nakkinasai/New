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
	     stage("deploy"){
		     steps{
			sshagent(['deploy_user']) {
	                    sh "scp -o StrictHostkeyChecking=no MyWebApp/target/MyWebApp.war ec2-user@13.126.76.37:/tomcat/ Apache Tomcat/9.0.82/MyWebApps"
			  
				  
 }
}

}
 }

}

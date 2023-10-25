pipeline
{
    agent any 
    parameters {
        choice(choices: 'Dev\nprod', description: 'Select Branch to build', name: 'branch')
    }
    environment {
        CI=true
        //branchName= 'env.BRANCH_NAME'
        //devRepoKey = Mini_Asg_02_Dev
        //prodRepoKey = Mini_Asg_02_Prod
        //ARTIFACTORY_ACCESS_TOKEN=credentials('Artifactory')  
        Tomcat1_Cat="/root/tomcat1/tomcat/bin"
        Tomcat2_Cat="/root/tomcat2/tomcat/bin"
        TARGET='/var/lib/jenkins/.m2/repository/com/hello/MyWebApp/0.0.1-SNAPSHOT/MyWebApp-0.0.1-SNAPSHOT.war'
       
        WEBAPP_NAME = 'MyWebApp' 
        //WEBAPP_NAME = 'Mini_Asg_02'
    }
    tools{
        maven "maven3"
    }
    stages {
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
		        withSonarQubeEnv(credentialsId: 'create-sonar') { 
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
        stage("Tomcat Deployment"){
            steps{
                script{
                    if (params.branch == "Dev"){
                         def TOMCAT_HOME = '/root/tomcat1/tomcat/webapps'
                         sh 'sudo cp ${TARGET} ${TOMCAT_HOME}/${WEBAPP_NAME}.war'
                         sh 'sudo ${Tomcat1_Cat}/shutdown.sh'
                         sh 'sudo ${Tomcat1_Cat}/startup.sh'
                         
                    }
                    else if(params.branch == "prod"){
                        def  TOMCAT_HOME = '/root/tomca2/tomcat/webapps'
                        sh 'sudo cp ${TARGET} ${TOMCAT_HOME}/${WEBAPP_NAME}.war'
                         sh 'sudo ${Tomcat2_Cat}/shutdown.sh'
                         sh 'sudo ${Tomcat2_Cat}/startup.sh'
                    }
                   
                }
            }
        }
    }
post {
            always {
                 //Send email on both success and failure
                script{
                        def subject = "Pipeline ${currentBuild.result}: ${currentBuild.fullDisplayName}"
                        def body = "Pipeline ${currentBuild.result}: ${currentBuild.fullDisplayName}\n\n" +
                                   "Build URL : ${env.BUILD_URL}\n" +
                                   "Project : ${env.JOB_NAME}\n" +
                                   "Branch : ${params.Environment}\n"
        
                        if (currentBuild.resultIsBetterOrEqualTo("SUCCESS")) {
                             //Successful build
                                mail to :"sainakkina15009@gmail.com",
                                subject: subject ,
                                body :body
                            
                        }
                        else {
                             //Failed build
                                mail to :"sainakkina15009@gmail.com",
                                subject: subject ,
                                body :body
                            }
                }
            }
        }
}

pipeline {
    agent any


    stages {
        stage('Clean Compile') {
            steps {
                
                // Clean and compile.
                sh "mvn clean compile"

            }
        }
        
        stage('Test') {
            steps {
                
                // Test Cases.
                sh "mvn test"

            }
        }
       
                
        stage('Install') {
            steps {
                
                
                sh "mvn install"

            }
        }
        
       /* 
        stage('Artifact'){
            steps{
                    archiveArtifacts 'target/*.war'
                }
            }
            
        stage ('Artifactory Configuration') {
            steps {
                rtServer (
                    id: "artifactory", 
                    url: "http://192.168.0.109:8081/artifactory",

                )

                rtMavenResolver (
                    id: 'maven-resolver',
                    serverId: 'artifactory',
                    releaseRepo: 'libs-release',
                    snapshotRepo: 'libs-snapshot'
                )  
                 
                rtMavenDeployer (
                    id: 'maven-deployer',
                    serverId: 'artifactory',
                    releaseRepo: 'libs-release-local',
                    snapshotRepo: 'libs-snapshot-local'
                )
            }
        }
        
        stage('upload') {
           steps {
              script { 
                 def server = Artifactory.server 'artifactory'
                 def uploadSpec = """{
                    "files": [{
                       "pattern": "/var/lib/jenkins/workspace/FirstJenkins_project/target/mvc_1.war",
                       "target": "project-libs-snapshot-local"
                    }]
                 }"""

                 server.upload(uploadSpec) 
               }
            }
        }

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "artifactory"
                )
            }
        }
        */
         stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t web .'
                }
            }
        }
        
     stage ('Publish ECR') {
       steps {
         withEnv (["AWS_ACCESS_KEY_ID=${env.AWS_ACCESS_KEY_ID}", "AWS_SECRET_ACCESS_KEY=${env.AWS_SECRET_ACCESS_KEY}", "AWS_DEFAULT_REGION=${env.AWS_DEFAULT_REGION}"]) {
           sh 'docker login -u AWS -p $(aws ecr get-login-password --region ap-northeast-1) 876724398547.dkr.ecr.ap-northeast-1.amazonaws.com'
           sh 'docker build -t web .'
           sh 'docker tag web:latest 876724398547.dkr.ecr.ap-northeast-1.amazonaws.com/web:""$BUILD_ID""'
           sh 'docker push 876724398547.dkr.ecr.ap-northeast-1.amazonaws.com/web:""$BUILD_ID""'
        }
        }
      }
      
      stage('Ec2-deploy'){
        
        steps{
            sh 'ssh -o "StrictHostKeyChecking=no" -i "key1.pem" ec2-user@3.112.32.71 docker login -u AWS -p $(aws ecr get-login-password --region ap-northeast-1)'
            sh 'ssh -o "StrictHostKeyChecking=no" -i "key1.pem" ec2-user@3.112.32.71 docker pull 876724398547.dkr.ecr.ap-northeast-1.amazonaws.com/ecr-demo:78'
            sh 'ssh -o "StrictHostKeyChecking=no" -i "key1.pem" ec2-user@3.112.32.71 docker run -d -p 8087:8080 876724398547.dkr.ecr.ap-northeast-1.amazonaws.com/ecr-demo:78'
           
        }   

        } 
        
    }
      }
node {
    try{
	SendSlackNotifications('STARTED')
	def mavenHome = tool name: 'maven 3.9.9'
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '3', numToKeepStr: '3')), pipelineTriggers([pollSCM('H/10 * * * *')])])
deleteDir()    
	stage('checkout'){
        git 'https://github.com/Darshanputtaswamy7/maven-web-application.git'
    }//checkout
    
    stage('build'){
        sh "${mavenHome}/bin/mvn clean package sonar:sonar deploy"
    }//build
	
	stage('deploy'){
        deploy adapters: [tomcat9(alternativeDeploymentContext: '', credentialsId: '225886a6-10e5-4487-a1ca-560c2927d323', path: '', url: 'http://13.204.69.235:8080/')], contextPath: null, war: '**/*.war'
    }
	}//try
catch (e)
{
currentBuild.result="Failed"
throw e 
}
finally{
SendSlackNotifications(currentBuild.result)	
}
}//node

def SendSlackNotifications(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESS'

// Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESS') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary)
}
	



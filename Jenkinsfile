node {
    
	try{
	
	SendSlackNotifications('STARTED')
	def mavenHome = tool name:'maven 3.9.9'
	properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '5', numToKeepStr: '5')), pipelineTriggers([githubPush()])])

    stage('checkout'){
     git 'https://github.com/Darshanputtaswamy7/maven-web-application.git'   
    }//checkout
    
    stage('build'){
        sh "${mavenHome}/bin/mvn clean package"
    }//build
    
    stage('deploy'){
        sh "${mavenHome}/bin/mvn clean sonar:sonar deploy"
    }//deploy
	
	stage('tomcat'){
        sshagent(['20b970f1-a034-4f9a-93ef-3d693af04418']){
   sh  "scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/piprlinecriptedway/target/maven-web-application.war ec2-user@172.31.15.177:/opt/apache-tomcat-9.0.104/webapps" 
}//ssh
    }//tomcat
catch (e)
{
currentBuild.result="Failed"
throw e 
}
finally{
SendSlackNotifications(currentBuild.result)	
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

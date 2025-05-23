pipeline {

agent any
tools {
  maven 'maven 3.9.9'
}//tools

triggers {
  githubPush()
}

options {
  buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '3', numToKeepStr: '3')
}
stages{
stage('checkout'){
steps{
     SendSlackNotifications('STARTED')  
git 'https://github.com/Darshanputtaswamy7/maven-web-application.git'
}
}

stage('build'){
steps{
sh "mvn clean package"
}
}
}//stages
post {
  success {
	deploy adapters: [tomcat9(alternativeDeploymentContext: '', credentialsId: 'd032a93b-3330-4e45-8300-b57f69dce8b4', path: '', url: 'http://172.31.14.242:8080/')], contextPath: null, war: '**/*.war'
     SendSlackNotifications(currentBuild.result)  
    // One or more stepss need to be included within each condition's block.
  }//success
  failure {
       SendSlackNotifications(currentBuild.result)
    // One or more stepss need to be included within each condition's block.
  }//failure
}//post

}//pipeline

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

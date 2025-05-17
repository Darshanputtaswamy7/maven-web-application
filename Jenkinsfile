pipeline {

agent any

tools {
  maven 'maven 3.9.9'
}


}

options {
  buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '3', numToKeepStr: '3')
  timestamps()
}
stages {

stage('checkout'){

steps{
    SendSlackNotifications('STARTED')
git branch: 'development', url: 'https://github.com/Darshanputtaswamy7/maven-web-application.git'
}//git
}//checkout

stage('parallel'){
steps{
parallel(
RununitTestcases: {
sh "mvn clean test "},//test
Build:
{
sh "mvn clean package "}//package
)
}//steps
}//stage

stage('Deploy to Tomcat'){
steps{
deploy adapters: [tomcat9(alternativeDeploymentContext: '', credentialsId: '2fd3bff6-378f-4791-9a7e-0cfd6f63fc84', path: '', url: 'http://3.109.59.33:8080/')], contextPath: null, war: '**/*.war'
}//deploy
}//tomcat

}//stages

post {
  success {
     SendSlackNotifications(currentBuild.result)  
    // One or more steps need to be included within each condition's block.
  }//success
  failure {
       SendSlackNotifications(currentBuild.result)
    // One or more steps need to be included within each condition's block.
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

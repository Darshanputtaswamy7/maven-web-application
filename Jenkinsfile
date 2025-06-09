pipeline {
    agent any
    tools {
  maven 'maven 3.9.9'
}//
//
options {
  timestamps()
  buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '3', numToKeepStr: '3')
}
parameters {
  choice choices: ['master', 'development', 'QA'], description: 'Branches', name: 'BranchName'
  string defaultValue: 'Darshan', description: 'Builder', name: 'Name'
}

stages {
   
   stage('checkout') {
  steps {
buildName "Darshan-${env.BUILD_DISPLAY_NAME}"
git branch: "${params.BranchName}", url: 'https://github.com/Darshanputtaswamy7/maven-web-application.git'
    // One or more steps need to be included within the steps block.
  }
   }
    
stage('build') {
  steps {
      parallel(
          Rununittestcases: {
      SendSlackNotifications('STARTED')
      sh "mvn clean test" },
      build:{
          sh "mvn package deploy"
      }
      )
  }
}
}//stages
post {
  success {
      SendSlackNotifications(currentBuild.result)
      deploy adapters: [tomcat9(alternativeDeploymentContext: '', credentialsId: '225886a6-10e5-4487-a1ca-560c2927d323', path: '', url: 'http://13.204.69.235:8080/')], contextPath: null, war: '**/*.war'
      }
      failure {
    SendSlackNotifications(currentBuild.result)
  }
}
}
def SendSlackNotifications(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESS'

// Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
    def subject = "${buildStatus}: Job '${env.JOB_NAME} [${currentBuild.displayName}]'"
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

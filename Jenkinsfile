node{
try{
notifyBuild('STARTED')
properties([
    buildDiscarder(logRotator(daysToKeepStr: '5', numToKeepStr: '5')),
    pipelineTriggers([githubPush()])
  ])
    def mavenHome = tool name: 'maven 3.9.9'
    stage('Checkoutcode'){git branch: 'development', url: 'https://github.com/Darshanputtaswamy8/maven-web-application.git'}
/*stage('Build'){
sh "${mavenHome}/bin/mvn clean package sonar:sonar deploy"
}
*/
stage('Build'){
sh "${mavenHome}/bin/mvn clean package sonar:sonar deploy"
}
stage('DeploytoTomcat'){
    sshagent(['a12a3370-c50d-4c33-8392-9d7e8623956c']){
    sh "scp -o StrictHostKeyChecking=no target/*.war ec2-user@172.31.15.177:/opt/apache-tomcat-9.0.104/webapps"
}
}

}
catch (e) {
    // If there was an exception thrown, the build failed
    currentBuild.result = "FAILED"
    throw e
  } finally {
    // Success or failure, always send notifications
    notifyBuild(currentBuild.result)
  }
}

def notifyBuild(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESSFUL'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME}' BUild#: '${env.BUILD_NUMBER}'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESSFUL') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary)
}

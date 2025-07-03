timestamps {
    
    node {

try {

    def mvnhome = tool name: 'Maven 3.9.10', type: 'maven'
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '3')), pipelineTriggers([githubPush()])])
    //buildName 'Darshan- ${env.BUILD_DISPLAY_NAME}'
    buildName "Darshan- ${env.BUILD_NUMBER}"
    stage('Checkout') {
        slackSend color: 'FFFF00', message: "${env.JOB_NAME} -  #${env.BUILD_DISPLAY_NAME} started by ${currentBuild.getBuildCauses()[0].userId} <${env.BUILD_URL}|Open> "
        git 'https://github.com/Darshanputtaswamy7/maven-web-application.git'
    }//checkout

    stage('Build') {
        sh "${mvnhome}/bin/mvn clean test package"
    }//build

}//try

catch (e) {
    currentBuild.result = 'FAILURE'
    slackSend color: 'FF0000', message: "${env.JOB_NAME} -  #${env.BUILD_DISPLAY_NAME} failed <${env.BUILD_URL}|Open> "
    throw e
}//catch
 finally {
     if (currentBuild.result == null || currentBuild.result == 'SUCCESS') {
        slackSend color: '00FF00', message: "${env.JOB_NAME} -  #${env.BUILD_DISPLAY_NAME} SUCCESS <${env.BUILD_URL}|Open> "
        deploy adapters: [tomcat9(alternativeDeploymentContext: '', credentialsId: 'ebacaee9-4c9b-4ecf-80a3-5a21db047e3a', path: '', url: 'http://13.232.34.102:8080/')], contextPath: null, war: '**/*.war'
        cleanWs()
     }//if


}//finally

}//node

}//timestamps

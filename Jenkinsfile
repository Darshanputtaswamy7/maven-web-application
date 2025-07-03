pipeline {
    agent any
	
	tools {
  maven 'Maven 3.9.10'
}
	options {
timestamps()
  buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5')
}//
triggers {
  githubPush()
}
parameters {
  choice choices: ['master', 'development', 'QA'], description: 'Branches', name: 'BranchName'
}//


stages {

stage('checkout') {
  steps {
  
  git branch: "${params.BranchName}", url: 'https://github.com/Darshanputtaswamy7/maven-web-application.git'
  
    
  }
}

stage('build') {
  steps {
            parallel(
Rununittestcases: {
slackSend color: "#FFFF00", message: "Build Started: ${env.JOB_NAME} #: ${env.BUILD_NUMBER} ${env.BUILD_URL}"
sh "mvn test"},
packages: {
sh "mvn clean package"}
)
  }
}
}
post {
  success {
    
    deploy adapters: [tomcat9(alternativeDeploymentContext: '', credentialsId: 'edd24898-3f40-4874-a0a9-31555c507d6b', path: '', url: 'http://13.204.69.235:8080/')], contextPath: null, war: '**/*.war'
    slackSend color: "#00FF00", message: "Build Success: ${env.JOB_NAME} #: ${env.BUILD_NUMBER} ${env.BUILD_URL}"
    cleanWs()
  }
  failure {
  cleanWs()
  slackSend color: "#FF0000", message: "Build Failed: ${env.JOB_NAME} ${env.BUILD_NUMBER} ${env.BUILD_URL}"
  }
}


}
   

node{
properties([
    buildDiscarder(logRotator(daysToKeepStr: '5', numToKeepStr: '5')),
    pipelineTriggers([githubPush()])
  ])
    def mavenHome = tool name: 'maven 3.9.9'
    stage('Checkoutcode'){git branch: 'development', url: 'https://github.com/Darshanputtaswamy7/maven-web-application.git'}
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

node {
    def mavenHome = tool name:'maven 3.9.9'
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
	
}//node


timestamps {
node {
    def mvnHome = tool name: 'maven 3.9.10', type: 'maven'
	properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '3')), pipelineTriggers([githubPush()])])
    deleteDir()
		try {
        stage('checkout') {
            slackSend color: "#FFFF00", message: "Build Started: ${env.JOB_NAME} ${env.BUILD_NUMBER} ${env.BUILD_URL}"
            git 'https://github.com/Darshanputtaswamy7/maven-web-application.git'
        }

        stage('Build') {
            sh "${mvnHome}/bin/mvn clean package"
        }

        stage('Tomcat Deploy') {
        deploy adapters: [tomcat9(alternativeDeploymentContext: '', credentialsId: 'edd24898-3f40-4874-a0a9-31555c507d6b', path: '', url: 'http://13.204.69.235:8080')], contextPath: null, war: '**/*.war'
		}

        currentBuild.result = 'SUCCESS'  // Set explicitly (optional, usually Jenkins sets this)

    } catch (e) {
        currentBuild.result = 'FAILURE' // Let Jenkins know this build failed
        slackSend color: "#FF0000", message: "Build Failed: ${env.JOB_NAME} ${env.BUILD_NUMBER} ${env.BUILD_URL}"
        throw e
    } finally {
        if (currentBuild.result == null || currentBuild.result == 'SUCCESS') {
            slackSend color: "#00FF00", message: "Build Success: ${env.JOB_NAME} #: ${env.BUILD_NUMBER} ${env.BUILD_URL}"
        }
    }
 }
 }

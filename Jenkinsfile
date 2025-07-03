pipeline { // pipeline block start

    agent any
    
    tools {
        maven 'Maven 3.9.10'
    }

    options {
        timestamps()
        buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '3')
    }

    triggers {
        githubPush()
        pollSCM '*/59 * * * *'
    }

    parameters {
        choice choices: ['development', 'master', 'QA'], description: 'Branches', name: 'BranchName'
    }

    stages { // stages block start

        stage('checkout') { // checkout stage start
            steps {
                buildName "Darshan- ${env.BUILD_NUMBER}"
                slackSend color: 'FFFF00', message: "${env.JOB_NAME} -  #${env.BUILD_DISPLAY_NAME} started by ${currentBuild.getBuildCauses()[0].userId} <${env.BUILD_URL}|Open> "
                git branch: "${params.BranchName}", credentialsId: '7c90dc78-e088-4fd2-b6eb-7960981444fe', url: 'https://github.com/Darshanputtaswamy7/maven-web-application.git'
            }
        } // checkout stage end

        stage('build') { // build stage start
            steps {
                parallel (
                    cleanandtest: {
                        sh 'mvn clean test'
                    },
                    package: {
                        sh 'mvn package'
                    }
                )
            }
        } // build stage end

    } // stages block end

    post { // post block start
        success {
            slackSend color: '00FF00', message: "${env.JOB_NAME} -  #${env.BUILD_DISPLAY_NAME} SUCCESS <${env.BUILD_URL}|Open> "
            deploy adapters: [tomcat9(alternativeDeploymentContext: '', credentialsId: 'ebacaee9-4c9b-4ecf-80a3-5a21db047e3a', path: '', url: 'http://13.232.34.102:8080/')], contextPath: null, war: '**/*.war'
            cleanWs()
        }
        failure {
            slackSend color: 'FF0000', message: "${env.JOB_NAME} -  #${env.BUILD_DISPLAY_NAME} failed <${env.BUILD_URL}|Open> "
        }
    } // post block end

} // pipeline block end

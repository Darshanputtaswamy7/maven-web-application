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
git 'https://github.com/Darshanputtaswamy7/maven-web-application.git'
}
}

stage('build'){
steps{
sh "mvn clean package"
}
}
}//stages
}//pipeline

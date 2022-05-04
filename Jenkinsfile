@Library("app-lib") _
pipeline {
  agent any

  tools {
    maven 'maven3'
  }
  options {
    buildDiscarder logRotator(daysToKeepStr: '10', numToKeepStr: '7')
  }
  parameters {
    choice choices: ['develop', 'qa', 'master'], description: 'Choose the branch to build', name: 'branchName'
  }
  stages {
    stage('Maven Build') {
      steps {
        sh 'mvn clean package'
      }
    }
    stage('Deploy to Tomcat') {
      steps {
        tomcatDeploy(["172.31.21.176","172.31.21.176","172.31.21.176"],"ec2-user","tomcat-dev")
      }
    }
  }
  post {
    success {
      archiveArtifacts artifacts: 'target/*.war'
      cleanWs()
    }
  }
}

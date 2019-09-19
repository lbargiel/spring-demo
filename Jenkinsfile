pipeline {
    agent any
    options {
        timestamps()
  ansiColor('xterm')
    }
    stages {
   stage('Preparation') {
       steps{
      git 'https://github.com/lbargiel/spring-demo.git'
       }
   }
   stage('Build') {
       steps {
     sh './mvnw clean package'
       }
   }
   stage('Results') {
      steps {
      junit '**/target/surefire-reports/TEST-*.xml'
      archiveArtifacts 'target/*.jar'
      }
   }
   stage('Docker image') {
    steps {
      sh './mvnw jib:dockerBuild'
    }

   }
    }
}
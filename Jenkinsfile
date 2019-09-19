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
     sh "./mvnw -Dimage=${currentBuild.number} clean package"
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
      sh '''
      eval eval $(minikube docker-env)
      ./mvnw jib:dockerBuild'''
    }

   }
    }
}
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
     sh "./mvnw clean package"
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

   stage('Deploy to test') {
    steps {
      sh '''
      kubectl create deployment spring-demo --image=spring-demo:0.0.1-SNAPSHOT
      kubectl expose deployment spring-demo --type=NodePort --port=8090
      '''
    }
   }
    }
}
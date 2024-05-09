pipeline {
   agent any

   tools {
      maven 'maven3'  // Specify the Maven tool defined in Jenkins
   }

   stages {
      stage('maven-buildstage') {
         steps {
            script {
               def mvnHome = tool name: 'maven3', type: 'maven'
               sh "${mvnHome}/bin/mvn clean package"
               sh 'mv target/myweb*.war target/newapp.war'
               }
            }
      }
   }
   
   stage('Build Docker Image'){
   sh 'docker build -t mlogu6/myweb:0.0.2 .'
   }
   
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u mlogu6 -p ${dockerPassword}"
    }
   sh 'docker push mlogu6/myweb:0.0.2'
   }
}

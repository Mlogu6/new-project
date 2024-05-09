pipeline {
   agent any
   environment {
      TAG = "v0.${env.BUILD_NUMBER}"
   }

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

         stage('Build Docker Image') {
            steps {
                // install docker in the jenkins server
                // execute this command to give the permission to build the image "chmod 777 /var/run/docker.sock"
                sh 'docker build -t mlogu6/myweb:${TAG} .'
            }
        }

         stage('Docker Image Push') {
            steps {
                withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
                    sh "docker login -u mlogu6 -p ${dockerPassword}"
                }
                sh 'docker push mlogu6/myweb:${TAG}'
            }
        }

         stage('Delete Images & Container') {
            steps {
                sh 'docker image prune --all --force && docker rm -f myproject'
            }
         }

         stage (Deployment) {
            steps {
                sh 'docker run -d -p 8091:8080 --name myproject mlogu6/myweb:${TAG}'
            }
        }
   }
}

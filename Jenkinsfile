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
}

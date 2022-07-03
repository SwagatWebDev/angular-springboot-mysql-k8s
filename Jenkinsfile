pipeline {
    agent any

    tools {
        maven "my-maven"
        nodejs "my-nodejs"
    }

    stages {

       stage('Angular Build Docker Image') {
           steps {
            dir("${env.WORKSPACE}/angular8-crud-demo-master") {
                script {
                    echo 'Angular Application Build Docker Image'
                    sh 'docker build -t swagatkm/angular-app:latest .'
                   }
                 }
               }
           }
       stage('Maven Build' ) {
            steps {
                dir("${env.WORKSPACE}/springboot-angular-kubernetes-master") {
                    echo 'SpringBoot CRUD Application Maven Build'
                    sh "mvn clean install"
                    sh "mvn package"
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
    }
}

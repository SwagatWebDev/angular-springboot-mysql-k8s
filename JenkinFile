pipeline {
    agent any

    tools {
        maven "my-maven"
    }

    stages {
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

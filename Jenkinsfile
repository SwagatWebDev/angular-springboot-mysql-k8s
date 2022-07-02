pipeline {
    agent any

    tools {
        maven "my-maven"
    }

    stages {
       stage('Maven Build' ) {
            steps {
                echo 'Maven Build'
                sh 'cd springboot-angular-kubernetes-master'
                sh "mvn clean install"
                sh "mvn package"
                archiveArtifacts 'target/*.jar'
            }
        }
    }
}

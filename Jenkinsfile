pipeline {
    agent any

    tools {
        maven "my-maven"
    }

    stages {
       stage('Maven Build' ) {
            steps {
                echo 'Maven Build'
                sh 'cd /home/jenkins/agent/workspace/Angular SpringBoot K8s Project/springboot-angular-kubernetes-master'
                sh "mvn clean install"
                sh "mvn package"
                archiveArtifacts 'target/*.jar'
            }
        }
    }
}

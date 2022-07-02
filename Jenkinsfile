pipeline {
    agent any

    tools {
        maven "my-maven"
    }

    stages {
       stage('Maven Build' ) {
            steps {
                echo 'SpringBoot CRUD Application Maven Build'
                sh 'cd angular8-crud-demo-master'
                sh "mvn clean install"
                sh "mvn package"
                archiveArtifacts 'target/*.jar'
            }
        }
    }
}

pipeline {
    agent any

    tools {
        maven "my-maven"
        nodejs "my-nodejs"
    }

    stages {

       stage('NPM Build') {
           steps {
               dir("${env.WORKSPACE}/angular8-crud-demo-master") {
                    echo 'Angular Project Build'
                    sh "npm install"
                    sh 'npx ng build --prod --base-href=/angular-test-code/ && cd dist/angular-test-code && jar -cvf angular-test-code.war *'
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

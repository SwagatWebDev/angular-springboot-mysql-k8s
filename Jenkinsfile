pipeline {

    options {
        ansiColor('xterm')
      }

    agent {
        kubernetes {
          yamlFile 'builder.yaml'
        }
      }

    tools {
        maven "my-maven"
        nodejs "my-nodejs"
    }

    stages {

       stage('Angular Docker Image Push') {
           steps {
               dir("${env.WORKSPACE}/angular8-crud-demo-master") {
               container('kaniko') {
                     script {
                       sh '''
                       /kaniko/executor --dockerfile `pwd`/Dockerfile \
                                        --context `pwd` \
                                        --destination=swagatkm/angular-app:latest
                       '''
                       }
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
       stage('SpringBoot Docker Image Push') {
             steps {
                 container('kaniko') {
                 dir("${env.WORKSPACE}/springboot-angular-kubernetes-master") {
                       script {
                         sh '''
                         /kaniko/executor --dockerfile `pwd`/Dockerfile \
                                          --context `pwd` \
                                          --destination=swagatkm/springboot-k8s-deployment:latest
                         '''
                         }
                     }
                 }
             }
        }
       stage('Deploy SpringBoot App to Kubernetes') {
             steps {
               container('kubectl') {
               dir("${env.WORKSPACE}/springboot-angular-kubernetes-master") {
                 withCredentials([file(credentialsId: 'mykubeconfig', variable: 'KUBECONFIG')]) {
                   sh 'kubectl --insecure-skip-tls-verify apply -f mysqldb-root-credentials.yml'
                   sh 'kubectl --insecure-skip-tls-verify apply -f mysqldb-credentials.yml'
                   sh 'kubectl --insecure-skip-tls-verify apply -f mysql-deployment.yml'
                   sh 'kubectl --insecure-skip-tls-verify apply -f mysql-configmap.yml'
                   sh 'kubectl --insecure-skip-tls-verify apply -f deployment.yml'
                 }
               }
             }
           }
         }
       stage('Deploy Angular App to Kubernetes') {
             steps {
               container('kubectl') {
               dir("${env.WORKSPACE}/angular8-crud-demo-master") {
                 withCredentials([file(credentialsId: 'mykubeconfig', variable: 'KUBECONFIG')]) {
                   sh 'kubectl --insecure-skip-tls-verify apply -f deployment.yml'
                 }
               }
             }
           }
    }
}

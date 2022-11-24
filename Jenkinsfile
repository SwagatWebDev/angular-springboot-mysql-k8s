pipeline {
    agent any

    tools {
        maven 'Maven3'
    }

    environment {
        registry-api = "120761001082.dkr.ecr.us-east-2.amazonaws.com/my-repo"
        registry-ui = "120761001082.dkr.ecr.us-east-2.amazonaws.com/my-repo-ui"
    }

    stages {
        stage('Code Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/swagat030/angular-springboot-mysql-k8s.git']]])
            }
        }

//         stage ('Maven Build: API') {
//             steps {
//                 dir("${env.WORKSPACE}/springboot-angular-kubernetes-master") {
//                     echo 'SpringBoot CRUD Application Maven Build'
//                     sh "mvn clean install"
//                     sh "mvn package"
//                     archiveArtifacts 'target/*.jar'
//                 }
//             }
//         }

        stage ('Building Docker Image: UI') {
            steps {
                dir("${env.WORKSPACE}/angular8-crud-demo-master") {
                    script {
                        demo = docker.build registry-ui
                    }
                }
            }
        }

//         stage('Building Docker Image: API') {
//             steps{
//                 dir("${env.WORKSPACE}/springboot-angular-kubernetes-master") {
//                     script {
//                         dockerImage = docker.build registry-api
//                     }
//                 }
//             }
//         }
//
//         stage('Pushing to ECR: API') {
//             steps {
//                 dir("${env.WORKSPACE}/springboot-angular-kubernetes-master") {
//                     script {
//                         sh 'aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin 120761001082.dkr.ecr.us-east-2.amazonaws.com'
//                         sh 'docker push 120761001082.dkr.ecr.us-east-2.amazonaws.com/my-repo:latest'
//                     }
//                 }
//             }
//         }

        stage('Pushing to ECR: UI') {
            steps {
                dir("${env.WORKSPACE}/angular8-crud-demo-master") {
                    script {
                        sh 'aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin 120761001082.dkr.ecr.us-east-2.amazonaws.com'
                        sh 'docker push 120761001082.dkr.ecr.us-east-2.amazonaws.com/my-repo-ui:latest'
                    }
                }
            }
        }

//         stage('K8S Deploy: API') {
//             steps {
//                 dir("${env.WORKSPACE}/springboot-angular-kubernetes-master") {
//                     script {
//                         withKubeConfig([credentialsId: 'k8s', serverUrl: '']) {
//                             sh ('kubectl apply -f  kubernetes/deployment.yml')
//                         }
//                     }
//                 }
//             }
//         }

    }
}

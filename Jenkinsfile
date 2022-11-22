pipeline {
    agent any

    tools {
        maven 'Maven3'
    }

    environment {
        registry = "120761001082.dkr.ecr.us-east-2.amazonaws.com/my-repo"
    }

    stages {
        stage('Code Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/swagat030/angular-springboot-mysql-k8s.git']]])
            }
        }

        stage ('Maven Build') {
            steps {
                dir("${env.WORKSPACE}/springboot-angular-kubernetes-master") {
                    echo 'SpringBoot CRUD Application Maven Build'
                    sh "mvn clean install"
                    sh "mvn package"
                    archiveArtifacts 'target/*.jar'
                }
            }
        }

        stage('Building Docker Image: API') {
            steps{
                dir("${env.WORKSPACE}/springboot-angular-kubernetes-master") {
                    script {
                        dockerImage = docker.build registry
                    }
                }
            }
        }

        stage('Pushing to ECR: API') {
            steps{
                dir("${env.WORKSPACE}/springboot-angular-kubernetes-master") {
                    script {
                        sh 'aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin account_id.dkr.ecr.us-east-2.amazonaws.com'
                        sh 'docker push account_id.dkr.ecr.us-east-2.amazonaws.com/my-docker-repo:latest'
                    }
                }
            }
        }

    }
}

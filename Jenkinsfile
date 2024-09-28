pipeline {
    agent any

    tools{
        jdk 'Java17'
        maven 'Maven3'
    }

    stages {


        stage('Build Maven') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/ferhataydin/devops-002-pipeline']])

                // sh 'mvn clean install'
                  bat 'mvn clean install'
            }
        }


        stage('Unit Test') {
            steps {
                // sh 'mvn test'
                  bat 'mvn test'

                // sh 'echo Unit Test'
                // bat 'echo Unit Test'
            }
        }


        stage('Docker Image') {
           steps {
               //  sh 'docker build  -t ferhataydin/devops-jenkins-app:latest  .'
                  bat 'docker build  -t ferhataydin/devops-jenkins-app:latest  .'
           }
        }


        stage('Docker Image to DockerHub') {
            steps {
                script{
                    withCredentials([string(credentialsId: 'DockerHub', variable: 'DockerHub')]) {

                        //  sh 'echo docker login -u ferhataydin -p DOCKERHUB_TOKEN'
                        // bat 'echo docker login -u ferhataydin -p DOCKERHUB_TOKEN'

                        // sh 'echo docker login -u ferhataydin -p ${dockerhub}'
                          bat 'echo docker login -u ferhataydin -p ${dockerhub}'

                        // sh 'docker image push  ferhataydin/devops-jenkins-app:latest'
                           bat 'docker image push  ferhataydin/devops-jenkins-app:latest'
                    }
                }
            }
        }


        stage('Deploy to Kubernetes'){
            steps{
                kubernetesDeploy (configs: 'deployment-service.yml', kubeconfigId: 'kubernetes' namespace: 'devops-jenkins-app')
            }
        }


       stage('Docker Image to Clean') {
           steps {
               //   sh 'docker rmi ferhataydin/devops-jenkins-app:latest'
               //  bat 'docker rmi ferhataydin/devops-jenkins-app:latest'

               // sh 'docker image prune -f'
                bat 'docker image prune -f'
           }
       }


    }
}

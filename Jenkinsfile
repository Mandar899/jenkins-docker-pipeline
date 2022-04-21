
pipeline {
    agent any
    environment{
        DOCKERHUBPWD = credentials('dockerhubpwd')
    }

    stages {
        stage('Git Clone'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github-private-key', url: 'https://github.com/Mandar899/node-docker-login.git']]])
            }   
        }


        stage('Build Image'){
            steps{
                script{
                    dockerImage = docker.build "mavericck/node-login:latest"
                }
            }
        }

        stage('Push to dockerhub'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhubpwd')]) {
                        echo '"$dockerhubpwd" | docker login --username mavericck --password-stdin'
                        bat 'docker push mavericck/node-login:latest'
                    }
                    
                }
            }
        }

        stage('Deploying App to Kubernetes'){
            steps{
                script{
                    kubernetesDeploy(configs: "node-login.yml", kubeconfigId: "kubernetes")
                }
            }
        }
    }
}
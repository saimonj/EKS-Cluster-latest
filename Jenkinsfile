pipeline {

    agent any

    environment {
        AWS_REGION = 'us-east-1'
    }

    stages {

        stage('Checkout application code') {
            steps {
                script {
                    checkout scmGit(
                        branches: [[name: '*/main']],
                        extensions: [],
                        userRemoteConfigs: [[
                            credentialsId: 'github-pat',
                            url: 'https://github.com/arunawsdevops/EKS-Cluster-latest.git'
                        ]]
                    )
                }
            }
        }

        stage('Terraform Init') {
            steps {
                script {
                    withAWS(credentials: 'aws_jenkins_key', region: 'us-east-1') {
                        sh '''
                        echo "------terraform init--------"
                        terraform init
                        '''
                    }
                }
            }
        }

        stage('Terraform Plan') {
            steps {
                script {
                    withAWS(credentials: 'aws_jenkins_key', region: 'us-east-1') {
                        sh '''
                        echo "------terraform plan--------"
                        terraform plan
                        '''
                    }
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                script {
                    withAWS(credentials: 'aws_jenkins_key', region: 'us-east-1') {
                        sh '''
                        echo "------terraform apply--------"
                        terraform apply -auto-approve
                        '''
                    }
                }
            }
        }

        stage('Terraform destroy') {    
            steps {
                script {
                    withAWS(credentials: 'aws_jenkins_key', region: 'us-east-1') {
                        sh '''
                        echo "------terraform apply--------"
                        terraform destroy -auto-approve
                        '''
                    }
                }
            }
        }
    }
}

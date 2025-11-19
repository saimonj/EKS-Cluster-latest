pipeline {

    agent any

    parameters {
        choice(
            name: 'ACTION',
            choices: ['apply', 'destroy'],
            description: 'Choose Terraform action to perform'
        )
    }

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
                    withAWS(credentials: 'aws_jenkins_key', region: AWS_REGION) {
                        sh '''
                        echo "------ terraform init --------"
                        terraform init
                        '''
                    }
                }
            }
        }

        stage('Terraform Plan') {
            steps {
                script {
                    withAWS(credentials: 'aws_jenkins_key', region: AWS_REGION) {
                        sh '''
                        echo "------ terraform plan --------"
                        terraform plan
                        '''
                    }
                }
            }
        }

        stage('Terraform Apply') {
            when {
                expression { params.ACTION == 'apply' }
            }
            steps {
                script {
                    withAWS(credentials: 'aws_jenkins_key', region: AWS_REGION) {
                        sh '''
                        echo "------ terraform apply --------"
                        terraform apply -auto-approve
                        '''
                    }
                }
            }
        }

        stage('Terraform Destroy') {
            when {
                expression { params.ACTION == 'destroy' }
            }
            steps {
                script {
                    withAWS(credentials: 'aws_jenkins_key', region: AWS_REGION) {
                        sh '''
                        echo "------ terraform destroy --------"
                        terraform destroy -auto-approve
                        '''
                    }
                }
            }
        }
    }
}

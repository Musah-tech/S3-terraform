pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout([
                    $class: 'GITSCM',
                    branches: [[name: "*/main"]],
                    extensions: [],
                    userRemoteConfigs: [[
                        url:'https://github.com/Musah-tech/S3-terraform.git'
                    ]]
                ])
            }
        }
        stage('Terraform Init') {
            steps {
                sh 'terraform init'
            }
        }
        stage('Plan Terraform') {
            step {
                script {
                    def planResult = sh(script: 'terraform plan -detailed-exitcode -out=tfplan', returnStatus: true)
                    if (planResult !=0 && planResult !=2) {
                        error "Terraform plan failed with exit code ${planResult}"
                    }
                }
            }
        }
        stage('Apply Terraform') {
            steps {
                sh 'terraform apply auto-approve tfplan'
            }
        }
    }
    post {
        always {
            sh 'rm -t tfplan'
        }
        sucess {
            echo 'Terraform deployment successful'
        }
        failure {
            echo 'Terraform deployment failed'
        }
    }
}   

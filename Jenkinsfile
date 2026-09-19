pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Terraform Format') {
            steps {
                bat 'terraform fmt -check -recursive'
            }
        }

        stage('Terraform Init') {
            steps {
                bat 'terraform init'
            }
        }

        stage('Terraform Validate') {
            steps {
                bat 'terraform validate'
            }
        }

        stage('TFLint') {
            steps {
                bat 'tflint --init'
                bat 'tflint'
            }
        }

        stage('Security Scan') {
            steps {
                bat 'trivy config . --severity HIGH,CRITICAL --exit-code 1'
            }
        }

        stage('Terraform Plan') {
            steps {
                bat 'terraform plan'
            }
        }

        stage('Approval') {
            steps {
                input message: 'Approve Terraform deployment?', 
                      ok: 'Deploy'
            }
        }

        stage('Terraform Apply') {
            steps {
                bat 'terraform apply -auto-approve'
            }
        }
    }

    post {
        success {
            echo 'CI/CD pipeline completed successfully.'
        }

        failure {
            echo 'Pipeline failed. Check the failed stage before deployment.'
        }
    }
}
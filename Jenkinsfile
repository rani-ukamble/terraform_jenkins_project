pipeline {
    agent any

    environment{
        GIT_REPO= 'https://github.com/rani-ukamble/terraform_jenkins_project'
        BRANCH='main'
        CLONE_DIR="my_floder"
        AWS_REGION = 'us-east-1' 
    }
  
  

    stages {
        stage('Checkout Code') {
            steps {
                 git branch:"${BRANCH}",url:"${GIT_REPO}"
            }
        }

        stage('Terraform Init') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'AWS_CREDENTIALS', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    sh 'terraform init'
                }
            }
        }

        stage('Terraform Validate') {
            steps {
                sh 'terraform validate'
            }
        }

        stage('Terraform Plan') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'AWS_CREDENTIALS', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    sh 'terraform plan -out=tfplan'
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                input "Approve to apply changes?"
                withCredentials([usernamePassword(credentialsId: 'AWS_CREDENTIALS', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    sh 'terraform apply tfplan'
                }
            }
        }
    }
}

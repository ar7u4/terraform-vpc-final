@Library('my-shared-library') _

pipeline{

    agent any

    parameters{

        choice(name: 'action', choices: 'create\ndelete', description: 'Choose create/Destroy')
        string(name: 'aws_account_id', description: " AWS Account ID", defaultValue: '295134731113')
        string(name: 'Region', description: "Region where resource need to be created", defaultValue: 'us-east-1')
        string(name: 'cluster', description: "name of the Terraform Services", defaultValue: 'demo-services')
        string(name: 'dirname', description: "directory name to create Resources")
    }
    environment{

        ACCESS_KEY = credentials('AWS_ACCESS_KEY_ID')
        SECRET_KEY = credentials('AWS_SECRET_KEY_ID')
    }

    stages{
         
        stage('Git Checkout'){
                    when { expression {  params.action == 'create' } }
            steps{
            gitCheckout(
                branch: "main",
                url: "https://github.com/ar7u4/jenkins-pipeline-terraform-var.git"
            )
            }
        }

        stage('Create Resources : Terraform'){
            when { expression {  params.action == 'create' } }
            steps{
                script{

                    dir('${params.dirname}') {
                      sh """
                          
                          terraform init 
                          terraform plan -var 'access_key=$ACCESS_KEY' -var 'secret_key=$SECRET_KEY' -var 'region=${params.Region}' --var-file=./config/terraform.tfvars
                          terraform apply -var 'access_key=$ACCESS_KEY' -var 'secret_key=$SECRET_KEY' -var 'region=${params.Region}' --var-file=./config/terraform.tfvars --auto-approve
                      """
                  }
                }
            }
        }

   
    }
}


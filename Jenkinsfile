pipeline { 
    agent any 
    environment { 
        AWS_ACCESS_KEY_ID     = credentials('jenkins-aws-secret-key-id') 
        AWS_SECRET_ACCESS_KEY = credentials('jenkins-aws-secret-access-key') 
    }
    options {
        ansiColor('xterm')
    }
    stages { 
        stage('Terraform Initialization') { 
            steps { 
                sh 'terraform init' 
                sh 'pwd' 
                sh 'ls -al' 
                sh 'printenv' 
            } 
        } 
        stage('Terraform Format') { 
            steps { 
                sh 'terraform fmt -check || exit 0' 
            } 
        } 
        stage('Terraform Validate') { 
            steps { 
                sh 'terraform validate'
            } 
        }
        stage('Terraform Planning') { 
            steps { 
                sh 'terraform plan -no-color -out=terraform_plan' 
            } 
        }
        stage('archive terrafrom plan output') {
            steps {
                archiveArtifacts artifacts: 'terraform_plan', excludes: 'output/*.md', onlyIfSuccessful: true
            }
        }
        stage('Terraform Apply') { 
            steps { 
                sh 'terraform apply -auto-approve'
            } 
        }
        stage('Terraform Destroy') { 
            steps {
                sh 'sleep 120'
                sh 'terraform destroy -auto-approve'
            } 
        }
    } 
}

pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID = credentials('aws-access-key')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-key')
        AWS_DEFAULT_REGION = 'us-east-1'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/devops-uk/ansible-aws.git'
            }
        }

        stage('Provision EC2') {
            steps {
                sh "ansible-playbook ec2-create.yml -i localhost"
            }
        }

        stage('Deploy HTTPD') {
            steps {
                sh "ansible-playbook -i aws_ec2.yml deploy-httpd.yml"
            }
        }
    }

    post {
        success {
            echo 'HTTPD deployed dynamically via Jenkins!'
        }
        failure {
            echo 'Deployment failed. Check logs.'
        }
    }
}


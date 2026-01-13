pipeline {
    agent any

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

        stage('Debug Workspace') {
            steps {
                sh 'pwd'
                sh 'ls -la'
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

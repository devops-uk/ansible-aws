pipeline {
    agent any

    environment {
        ANSIBLE_DIR = 'ansible-aws'
        INVENTORY = "${ANSIBLE_DIR}/aws_ec2.yml"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/devops-uk/ansible-aws.git'
            }
        }

        stage('Provision EC2') {
            steps {
                dir("${ANSIBLE_DIR}") {
                    sh "ansible-playbook ec2-create.yml -i localhost"
                }
            }
        }

        stage('Deploy HTTPD') {
            steps {
                dir("${ANSIBLE_DIR}") {
                    sh "ansible-playbook -i aws_ec2.yml deploy-httpd.yml"
                }
            }
        }

        stage('Debug Workspace') {
            steps {
                dir("${ANSIBLE_DIR}") {
                    sh 'pwd'
                    sh 'ls -la'
                }
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

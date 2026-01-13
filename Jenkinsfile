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
                sh "ansible-playbook ${ANSIBLE_DIR}/ec2-create.yml -i localhost"
            }
        }

        stage('Deploy HTTPD') {
            steps {
                sh "ansible-playbook -i ${INVENTORY} ${ANSIBLE_DIR}/deploy-httpd.yml"
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


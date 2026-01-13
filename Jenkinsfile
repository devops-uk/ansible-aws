pipeline {
    agent any

    environment {
        ANSIBLE_DIR = 'ansible-aws'
        INVENTORY = "${ANSIBLE_DIR}/inventory.ini"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/devops-uk/ansible-aws.git'
            }
        }

        stage('Run Ansible HTTPD Playbook') {
            steps {
                // Ensure Ansible is installed on Jenkins node
                sh 'ansible --version'

                // Run playbook
                sh "ansible-playbook -i ${INVENTORY} ${ANSIBLE_DIR}/deploy-httpd.yml"
            }
        }
    }

    post {
        success {
            echo 'HTTPD deployment successful!'
        }
        failure {
            echo 'Deployment failed. Check console output.'
        }
    }
}
